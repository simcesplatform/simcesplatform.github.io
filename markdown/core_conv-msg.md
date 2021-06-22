# Conventions of messaging

Please follow these conventions if you are either:

- specifying a new message structure
- developing an application that reads or creates messages

## MUST: Communication protocol

To communicate, the components MUST use AMQP 0-9-1 as the protocol. To implement this protocol, the components MUST use RabbitMQ.

## MUST: AMQP exchange type

To communicate, all components MUST use the exchange type "topic" in the message bus.

## MUST: Character encoding

All messages MUST be encoded in UTF-8. This is widely supported and also a common default in many environments.

The existing software tools of the platform expect UTF-8. If you use another encoding, you cause a risk of conflicts.

## MUST: Serialization in JSON

All message structures are serialized in JSON (JavaScript Object Notation). JSON is widely supported and platform-independent as well as human and machine readable.

The existing software tools of the platform expect JSON.

## MUST: Inherit from AbstractResult

When you create a result message type, you MUST inherit it from _AbstractResult_. This ensures that the platform can deliver and log the message as expected.

Please note that this only applies to full message structures. That is, any re-usable "blocks" that can be included in message types, such as _Time series block_, do not inherit from any base message type.

## SHOULD: Naming

When creating new items, a question arises how to formulate names. See the page [Conventions of naming](core_conv-name.md).

## SHOULD: Re-use existing structures where possible

The developer SHOULD avoid creating a new structure if there is a suitable existing structure. For example, if you want to include a timeseries in a message, use the _Time series block_.

For existing structures, see the page [Message structures](core_msg.md) the related pages under it.

## SHOULD: Time zone

When a message carries a time value, the time zone SHOULD always be UTC (Universal Coordinated Time). This prevents any conflicts due to daylight savings, which has conventionally been a difficult issue in software. Furthermore, the simulation platform may be distributed to multiple time zones. Finally, as the times are displayed to human users, it is straightforward to convert a UTC value to local time if required.

## SHOULD: Date, time and duration format

All date and time fields SHOULD follow the standard ISO 8601. This is both human and machine readable, and modern software tools have APIs to process ISO 8601. If absolutely necessary, it is straightforward to even implement a parser, especially as there is likely no requirement to support all the features.

The same applies to duration values.

## SHOULD: Units of measure

Certain values have an associated unit of measure. As these values are serialized as a message, the unit SHOULD be explicit and follow these rules: [Unit of measure (UCUM)](core_ucum.md).

## SHOULD: Omitting optional fields

If a field in a message structure is declared OPTIONAL, any software creating a message instance SHOULD omit the field when it has no relevant value. That is, the software SHOULD NOT assign null or an empty value instead.

Motivation: The semantics of an "optional" field truly refers to the possibility of omitting the field. However, if the value is empty instead, this can cause a question of how to interpret the value.

## SHOULD: Flags of AMQP queues

Each AMQP queue SHOULD use the following flags:

- Auto delete: true
- Exclusive: true

Auto delete means that the queue is deleted when no longer used. This enables the message bus to automatically clean up unused resources.

Exclusive means that no other client can use the queue. In the context of the simulation platform, shared queues are considered needless. The exclusive flag prevents any accidental queue sharing and supposedly leads to the queue being deleted when no longer used.

## SHOULD: Filter messages with topics

Whenever a sequence of published messages contains messages that only a subset of clients want, you SHOULD route the messages with multiple topics to avoid the need to filter messages after reception. Message routing with the message bus is efficient, because this is exactly the use case. Filtering SHOULD NOT take place in recipients, because they would have to first receive each message and then inspect the content to determine whether it is relevant or not.

For example, let us imagine that there is a topic called "PowerConsumption" that delivers power consumption values from buildings called House A, House B and House C. There is a recipient called Monitor X that only wants to monitor House A. For this, Monitor X must receive all messages and ignore those of House B and House C. Because this is inefficient, the "PowerConsumption" topic SHOULD be split to "PowerConsumption.HouseA", "PowerConsumption.HouseB" and "PowerConsumption.HouseC". This does not prevent a recipient from receiving all messages from the subtopics of "PowerConsumption", because wildcard subscriptions are possible.

Even if it were unclear if subtopics are needed, it is generally a good principle to separate messages. This is because the re-arrangement of topics can be expensive, whereas it is cheap to originally add more topics.
