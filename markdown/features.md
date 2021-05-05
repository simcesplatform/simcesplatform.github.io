
# Features

The platform provides the following features.

## Loose coupling of components
It is easier to manage a complex simulation system if the components only have the dependencies necessary for communication. The platform uses publish-subscribe communication, which decouples systems in "time, space and synchronisation" [1]. This means that the components do not interact directly but only via a communication platform. As the result, it is easier to develop and re-configure the setup as needed.

## Run simulation with single command
Once the environment has been set up, it is started with a single command. The platform takes care of starting and stopping the components.

## Analysis of results
The platform provides a logging system that captures all messages sent from a component. After the simulation, you can explore the messages either one by one or by generating time series from individual messages.

## Add your own components
In the platform, the simulation components have a workflow they must implement. To develop your own component, the sole requirement is to implement the required communication interface and follow the workflow.

## Parametrize your components
The platform provides a mechanism to deliver parameters to the components. You specify by yourself what the parameters are and what values these can have. For example, if your system has a storage of electricity or liquid with size as a property, you can simulate how the size affects the overall system.

## Geographical distribution
If needed, you can distribute the simulations geographically. You can connect a simulation component from anywhere in the world via Internet. This enables a joint effort together with project partners as well as the application of a server cluster located in other premises.

## Principles of messaging
The communication in based on a message bus. This is defined as follows:

> A Message Bus is a combination of a common data model, a common command set, and a messaging infrastructure to allow different systems to communicate through a shared set of interfaces. [2]

As the message bus is in place, the components do not interact directly. Instead, when a component has something to say, it publishes a message.

Because the bus uses the publish-subscribe paradigm, any other simulation component can subscribe for the messages to receive them. This is implemented with _topics_. A topic is an arbitrary string that identifies a subject of interest. The string can be, e.g., "MyRoom.MySensorA.Temperature" or "WeatherInfo.CityX". The topic-based approach realizes loose coupling, because the components do not directly connect to each other.

## Time and synchronization
The simulator components are distributed but should still operate together, which necessitates a mechanism to synchronize time. This is implemented with _epochs_. An epoch represents a period of simulated time, e.g., today between 12:00 and 12:15 p.m. The length of epoch can be varied depending on the desired resolution of simulation. As epochs always represent simulated time, the duration is different in real time. This depends mainly on how fast the slowest component can simulate.

For more information, please see the page Time and synchronization.

## Managing components in platform or externally
The simulation platform exploits virtualization to facilitate the management of components, but this has limitations. Virtualization is suitable for lightweight components that do not require particularly lot of computational power. On the other hand, sometimes a more powerful platform - possibly even a server cluster - is necessary. To enable both easy management and heavy computation, the platform supports two types of components: _platform managed_ and _externally managed_.

For more information, see Platform-managed and externally managed components.

## Accessing the results
During a simulation run, the logging system of the platform stores any messages exchanged between the simulation components. The logging system provides an HTTP API to access the messages. (In technical terms, this API is Restful, although not all HTTP-based technologies are.)

Features:

- View the results with any HTTP client, including a web browser or a custom client
- View individual messages
- Build timeseries from multiple messages of the same structure
- Retrieve data in:
- JSON (JavaScript Object Notation)
- CSV (Comma-separated Values; only for timeseries)


## References

[1] Patrick Th. Eugster, Pascal A. Felber, Rachid Guerraoui, and Anne-Marie Kermarrec. 2003. The many faces of publish/subscribe. ACM Comput. Surv. 35, 2 (June 2003), 114–131. [DOI:10.1145/857076.857078](https://doi.org/10.1145/857076.857078)

[2] Gregor Hohpe and Bobby Woolf, Enterprise Integration Patterns: Message Bus. [https://www.enterpriseintegrationpatterns.com/patterns/messaging/MessageBus.html](https://www.enterpriseintegrationpatterns.com/patterns/messaging/MessageBus.html)

