# Footprint of AMQP items

This page estimates the resource consumption of the routing-related items in RabbitMQ.


## Connections and channels

It is recommended to only have one connection per application and keep it alive. Channels should be re-used as well:

> The handshake process for an AMQP connection is quite involved and requires at least 7 TCP packets (more, if TLS is used). -- -- Don't open a channel each time you are publishing. The best practice is to reuse connections and multiplex a connection between threads with channels. You should ideally only have one connection per process, and then use a channel per thread in your application. [BestPrac1]


## Heartbeat

For RabbitMQ, the default heartbeat value is 60 seconds [Heartbeat]. Still, the value could/should be shorter:

> Several years worth of feedback from the users and client library maintainers suggest that values lower than 5 seconds are fairly likely to cause false positives, and values of 1 second or lower are very likely to do so. Values within the 5 to 20 seconds range are optimal for most environments. [Heartbeat]


## Queues

Queues must always accommodate at least the messages that have not yet been retrieved from the queue. This cannot be avoided.

When no longer needed, it is easy to clean up a queue with the "auto delete" flag that leads to the queue being removed once no client uses it anymore. Furthermore, the "exclusive" flag will have this effect as well. These should always be enabled, unless the use case does not work with these flags.

- Exclusive: "Exclusive queues may only be accessed by the current connection, and are deleted when that connection closes." [AmqpRef]
- Auto delete: "If set, the queue is deleted when all consumers have finished using it." [AmqpRef]


## Exchanges

Exchanges do consume some additional resources:

> Internal database (Mnesia) tables keep an in-memory copy of all its data (even on disc nodes). Typically this will only be large when there are a large number of queues, exchanges, bindings, users or virtual hosts. [Memory]

On the other hand:

> In Erlang, which RabbitMQ is built on, each node (broker) is a process, as is each queue. -- -- However, an exchange is not a process for scalability reasons, it is simply a row in RabbitMQ’s built-in Mnesia database. [Topol]

The conclusion is that exchanges are not expected to take a remarkable share of memory. Therefore, it is assumed that a "large" number of exchanges does not cause issues in a server. However, unused exchanges should eventually be removed.


## References

[AmqpRef] [https://www.rabbitmq.com/amqp-0-9-1-reference.html](https://www.rabbitmq.com/amqp-0-9-1-reference.html)

[BestPrac1] [https://www.cloudamqp.com/blog/2017-12-29-part1-rabbitmq-best-practice.html](https://www.cloudamqp.com/blog/2017-12-29-part1-rabbitmq-best-practice.html)

[Heartbeat] [https://www.rabbitmq.com/heartbeats.html](https://www.rabbitmq.com/heartbeats.html)

[Memory] [https://www.rabbitmq.com/memory-use.html](https://www.rabbitmq.com/memory-use.html)

[Topol] [https://spring.io/blog/2011/04/01/routing-topologies-for-performance-and-scalability-with-rabbitmq/](https://spring.io/blog/2011/04/01/routing-topologies-for-performance-and-scalability-with-rabbitmq/)
