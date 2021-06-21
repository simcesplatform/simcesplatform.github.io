# Platform-managed and externally managed components

The simulation platform exploits virtualization to facilitate the management of components, but this has limitations. In a simulation system, it is common to include components that do not calculate anything complex but implement a simple mathematical model. Some components are even mocks that supply pre-defined information, such as values from an earlier recorded or generated timeseries. Either way, computational power is not a particular requirement. Still, because the simulation platform is distributed and expects loose coupling, even simple components should have their own runtime and a service interface. To reduce the burden of this setup, virtualization with Docker provides a solution, because it enables the declaration of an autonomous computational unit with a few lines of code. Still, this does not suit for any resource-intensive computation, especially if the intention is to use a server cluster.

To gain the advantages of virtualization but still enable an arbitrary runtime and resource-intensive calculation, the simulation platform supports two types of components:

- **Platform-managed components** are executed in Docker in the same computer as the simulation platform. They are started for each simulation run and expected to stop execution when the simulation run ends.
- **Externally managed components** can be executed on any platform that can be either virtualized or not. The difference to platform managed is that the simulation platform does not control the startup and end of execution. Instead, the component must be running and connected to the message bus before any simulation can occur.

The tradeoff between platform managed and externally managed is easier management versus flexibility in computational capacity.