# Simulation Environment of Complex Energy System (SimCES)

Scientific simulation systems receive benefit from a support for distribution as well as the ease of operation and management, but this combination does not realize in earlier systems. Imagine a system where you can distribute the simulation task to multiple components and software platforms over a network but still manage the simulation as if you had a single piece of software. A distributed setup enables you to exploit the functionality of various platforms, combine the work of multiple simulator system developers and run simulation components in parallel if a lot of computational capacity is necessary. Besides, even real-life systems are distributed, so distribution enables you to model the world as it actually is. This increases the credibility of simulation results.

The earlier distributed simulation approaches are far from easy. Functional Mock-up Interface (FMI) is widely used but assumes you run all simulation components in one computer. Ad hoc networking has been applied, but this is laborious to deploy and maintain in large systems.

Finally, the analysis of simulation results is the ultimate aim of the system. There should be a means to access all of the simulation results, including the intermediate calculations of the simulation components.

SimCES (Simulation Environment of Complex Energy System) aims to fulfill these requirements. It does not develop any complex algorithms for you but provides a straightforward yet powerful concept for the management of simulation systems and the communication of simulation components.

From here, SimCES is referred to as the _platform_.
