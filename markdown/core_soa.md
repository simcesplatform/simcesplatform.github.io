# Service-oriented architecture

The simulation platform aims at facilitating the design and maintenance of components, so each developer SHOULD follow the principles of Service-oriented Architecture (SOA). SOA aims to realize a world where the pieces of systems are abstract services with few dependencies. In some cases, SOA can increase the work required to design an individual component, but this is compensated with easier maintenance and scalability during the lifecycle of the system.

SOA has a number of principles that the developers should follow. The following apply in the simulation platform.


## Loose coupling

"ensure that the service contract is not tightly coupled to the service consumers and to the underlying service logic and implementation" [https://en.wikipedia.org/wiki/Service_loose_coupling_principle](https://en.wikipedia.org/wiki/Service_loose_coupling_principle)

-> Do not let product- or runtime-specific features be visible in interfaces!


## Abstraction

"information published in a service contract is limited to what is required to effectively utilize the service"	[https://en.wikipedia.org/wiki/Service_abstraction](https://en.wikipedia.org/wiki/Service_abstraction)


## Reusability

"to create services that can be reused across a business" [https://en.wikipedia.org/wiki/Service_reusability_principle](https://en.wikipedia.org/wiki/Service_reusability_principle)


## Composability

"can be reused in multiple solutions that are themselves made up of composed services" [https://en.wikipedia.org/wiki/Service_composability_principle](https://en.wikipedia.org/wiki/Service_composability_principle)


## Autonomy

"to provide services with improved independence from their execution environments" [https://en.wikipedia.org/wiki/Service_autonomy_principle](https://en.wikipedia.org/wiki/Service_autonomy_principle)

-> Do not let product- or runtime-specific features be visible in interfaces!


## Standized service contract

"service contracts within a service inventory (enterprise or domain) adhere to the same set of design standards" [https://en.wikipedia.org/wiki/Standardized_service_contract](https://en.wikipedia.org/wiki/Standardized_service_contract)