# Management Exchange

Management Exchange enables a communication channel that is available even when no simulation is running.

To use the Exchange, the following information MUST match between all software applications. Otherwise, one or more applications will fail to use the exchange.

| Item | Value | Comment |
|-|-|-|
| Type        | topic | Necessary for topic-based communication |
| Name        | procem-management | Predefined name, so that any client can use the exchange |
| Durable     | true | The exchange will survive broker restart. This enables any clients to reconnect as soon as possible without a requirement to re-create the exchange. |
| Auto delete | false | The exchange will remain even if no client used it, as the management exchange should always be available regardless if a simulation is running or not. Please note that this value is independent of the "auto delete" flag of message queues! |
