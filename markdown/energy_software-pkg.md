# Software packages

To reduce the amount of redundant development work, there are re-usable software packages.


## Domain Messages

This package implements classes for processing messages that multiple components must use in communication.
These messages include at least ResourceState and PriceForecastState.

The package and its detailed documentation are available at **TODO**


## Matlab integration

To integrate Matlab with the RabbitMQ/AMQP message bus, you can use one of the following.

### AmqpMathToolIntegration

This Java library provides you a full control over when you check for new messages.
See [https://github.com/simcesplatform/AmqpMathToolIntegration](https://github.com/simcesplatform/AmqpMathToolIntegration)

### Cocop.AmqpMathToolConnector

This Java library delivers received messages as Matlab events as soon as they arrive.
See
[https://github.com/kannisto/Cocop.AmqpMathToolConnector](https://github.com/kannisto/Cocop.AmqpMathToolConnector)
