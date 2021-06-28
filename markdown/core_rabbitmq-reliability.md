# Reliable messaging in RabbitMQ

This page presents RabbitMQ mechanisms to increase the reliability of messaging. This is a summary from [https://www.rabbitmq.com/confirms.html](https://www.rabbitmq.com/confirms.html). The content has been gathered selectively.


## Reliable messaging needed in simulations?

AMQP connections are based on TCP (Transmission Control Protocol), which guarantees reliability as long as the connection remains open.
TCP is considered reliable enough for this platform without any additional mechanims.
If the connection dies in a node, the simulation fails anyway and this is detected as soon as notifications about finishing cease appearing.

Therefore, it is suggested to use automatic acks in RabbitMQ to reduce the burden of software development.
Still, nothing prevents a single developer from enabling additional mechanisms for reliability.


## Confirmations from consumers

### Automatic confirmations

"In automatic acknowledgement mode, a message is considered to be successfully delivered immediately after it is sent." That is, the broker will not retry if the consumer dies after a message is delivered from a queue to a consumer.

However, we still have an underlying TCP connection, which is supposedly reliable as long as it remains open.

### Manual confirmations

If a consumer applies this mode, it MUST acknowledge all deliveries after reception. The mode is more secure compared to automatic acks, but the consumer might still crash after ack.

#### Confirm types

##### ack

"used for positive acknowledgements"

##### reject

"used for negative acknowledgements"

Signals the broker that "delivery wasn't processed but still should be deleted".

##### nack

"used for negative acknowledgements"

This is a RabbitMQ extension to the AMQP protocol. Acks can be batched to reduce traffic. However, because "reject" does not support this, nack was introduced in RabbitMQ.

#### Requeue

Reject and nack have an option to ask the broker to requeue a non-delivered message. This is controlled with the "requeue" field.

### Channel prefetch

The "basic.qos" method enables setting the maximum number of messages that the broker can send to a consumer before receiving a confirmation for the first. This protects the consumers from receiving too many messages at once.


## Confirmations to publishers

RabbitMQ implements an extension to AMQP to enable "publisher confirms". Although TCP is a reliable protocol, there is a risk that a dead connection is detected only after various minutes (see [https://www.rabbitmq.com/heartbeats.html](https://www.rabbitmq.com/heartbeats.html) ).

Standard AMQP provides a mechanism called transactions, but this reduces throughput.