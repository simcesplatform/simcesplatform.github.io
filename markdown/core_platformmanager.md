# Platform Manager

- TODO: add links to manifest file page and simulation configuration file page
- TODO: add other documentation page links where appropriate

Component that is used to start new simulation runs in the SimCES simulation platform.
It generates and starts the Docker containers for the platform managed components in the new simulation run and publishes the simulation parameters using [Start message](core_msg-start.md) via [Management exchange](core_exchange-mgmt.md).

## Functionalities

- Parses the component manifest files to determine the available component types for a new simulation.
- Parses the simulation configuration file to determine the parameters for the new simulation.
- Creates the Docker containers for the required platform managed components for the new simulation.
- Starts the created Docker containers.
- Sends the [Start message](core_msg-start.md) via [Management exchange](core_exchange-mgmt.md) for the new simulation.

## Technical details

- Platform Manager is written in Python (3.7.9) and the code repository is found at: [https://github.com/simcesplatform/platform-manager](https://github.com/simcesplatform/platform-manager)
- Platform Manages connects to the Docker Engine of the host machine. All Docker containers created for simulations are created to Docker Engine of the host machine.
- Platform Manager connects to RabbitMQ message bus. Both local and remote message bus servers are supported.
- Platform Manager forwards the RabbitMQ and MongoDB connection details to those platform managed components that need them using environmental variables.
- One instance of Platform Manager can only start one simulation run. I.e., Platform Manager is given the simulation configuration at the startup and after finishing the simulation start procedures Platform Manager closes itself.

## Workflow

The workflow of a simulation is explained in page [Simulation Lifecycle](core_lifecycle.md).

Required before running Platform Manager to start a new simulation run:

- Docker installed and available for the Platform Manager
- A running Log Writer instance listening to the Management Exchange
    - Only required for logging the Start messages
- All required component manifest files available for the Platform Manager
- All required static files available for simulations
- All required Docker images available for the Platform Manager
- Connection parameters for RabbitMQ and MongoDB given in the configuration files for the Platform Manager

It is advices to start the Platform Manager using the provided start simulation script which can be used to easily provide the Platform Manager with the wanted simulation configuration.

Workflow of the Platform Manager instance once it has been started:

1. Parse the available component manifest files.
2. Parse the simulation configuration file.
3. Create new Docker containers for the platform-managed simulation components:
    - [Log writer](core_logwriter.md) instance for the simulation specific exchange
    - Platform managed components specified in the simulation configuration file
    - [Simulation Manager](core_simulationmanager.md) for the simulation run
4. Start the created Docker containers.
5. Send a Start message for the new simulation run via the Management exchange.
6. Close the Platform Manager instance.

## Environment variables

The parameters for the Platform Manager are given in 3 environment variable files: `common.env`, `rabbitmq.env` and `mongodb.env`. Some of the parameters are described here and all of these are also passed to the platform managed components with the exception that only the Log Writer receives the MongoDB parameters. For the parameters not described here there are description comments in the files themselves.

### `common.env`

| Variable name        | Default | Description |
| -------------------- | ------- | ----------- |
| SIMULATION_LOG_LEVEL | 20      | The logging level for which the logging messages are included in the output of the components. 30 to include only warnings and errors, 20 to include also info messages, and 10 to include the debug messages as well. |

### `rabbitmq.env`

| Variable name        | Default         | Description |
| -------------------- | --------------- | ----------- |
| RABBITMQ_HOST        | simces_rabbitmq | The host name for the RabbitMQ message bus. Can also be Docker container name for a container in the same Docker network as the Platform Manager. |
| RABBITMQ_PORT        | 5672            | The port number for the RabbitMQ message bus. |
| RABBITMQ_LOGIN       |                 | The username for the RabbitMQ message bus. |
| RABBITMQ_PASSWORD    |                 | The password for the RabbitMQ message bus. |
| RABBITMQ_SSL         | false           | Whether the connection to the RabbitMQ message bus is secured or not (true/false) |
| RABBITMQ_SSL_VERSION | PROTOCOL_TLS    | The security protocol used in the RabbitMQ message bus connection. Only considered if RABBITMQ_SSL is true. |

### `mongodb.env`

| Variable name                          | Default        | Description |
| -------------------------------------- | -------------- | ----------- |
| MONGODB_HOST                           | simces_mongodb | The host name for MongoDB. Can also be Docker container name for a container in the same Docker network as the Platform Manager. |
| MONGODB_PORT                           | 27017          | The port number for MongoDB. |
| MONGODB_USERNAME                       |                | The username for MongoDB. If this is empty, no access control is used. |
| MONGODB_PASSWORD                       |                | The password for MongoDB. |
| MONGODB_ADMIN                          | true           | Whether the account given in MONGODB_USERNAME has root access or not. Ignored if MONGODB_USERNAME is empty. (true/false) |
| MONGODB_TLS                            | false          | Whether the connection to MongoDB is secured or not (true/false) |
| MONGODB_TLS_ALLOW_INVALID_CERTIFICATES | false          | Whether to allow invalid security certificates. Ignored if MONGO_TLS is false. (true/false) |
