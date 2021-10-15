# Simulation Tools package

To reduce redundant development work, the package Simulation Tools implements re-usable software modules. The package has been implemented in Python.

For example, Simulation Tools includes following features:

- Classes for reading and creating messages, such as Epoch and Status
- Classes for creating domain-specific messages from AbstractMessage and AbstractResult
- Classes for processing re-usable message blocks, such as QuantityBlock and TimeSeriesBlock
- Network client class for communication with RabbitMQ message bus
- An abstract base class with the common functionality of simulation components
- Class for processing datetime values in ISO 8601
- Class for logging (locally within the component)
- Timer class

To access the software and view its detailed documentation, visit [https://github.com/simcesplatform/simulation-tools](https://github.com/simcesplatform/simulation-tools)
