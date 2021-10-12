# Configuring platform settings

- TODO: add general platform level settings section

## General platform level settings

## Simulation run specific settings

The simulation configuration YAML file is the main configuration for each simulation run. However, for the communication to work some environment settings must be configured properly.

The environment variables are passed to [Platform Manager](core_platformmanager.md) when a new simulation run is started. They are given in 3 environment variable files: `common.env` (for some common settings), `rabbitmq.env` (for the connection details for RabbitMQ) and `mongodb.env` (for connection details for MongoDB). Some of the parameters are described here. All of the common and RabbitMQ are then also passed to the platform managed components in the new simulation run. In addition, the simulation run specific Log Writer instance also received the MongoDB settings. For the parameters not described here there are description comments in the files themselves.

### `common.env`

Common settings for all simulation components:

| Variable name        | Default | Description |
| -------------------- | ------- | ----------- |
| SIMULATION_LOG_LEVEL | 20      | The logging level for which the logging messages are included in the output of the components. 30 to include only warnings and errors, 20 to include also info messages, and 10 to include the debug messages as well. |

### `rabbitmq.env`

RabbitMQ connection settings:

| Variable name        | Default         | Description |
| -------------------- | --------------- | ----------- |
| RABBITMQ_HOST        | simces_rabbitmq | The host name for the RabbitMQ message bus. Can also be Docker container name for a container in the same Docker network as the Platform Manager. |
| RABBITMQ_PORT        | 5672            | The port number for the RabbitMQ message bus. |
| RABBITMQ_LOGIN       |                 | The username for the RabbitMQ message bus. |
| RABBITMQ_PASSWORD    |                 | The password for the RabbitMQ message bus. |
| RABBITMQ_SSL         | false           | Whether the connection to the RabbitMQ message bus is secured or not (true/false) |
| RABBITMQ_SSL_VERSION | PROTOCOL_TLS    | The security protocol used in the RabbitMQ message bus connection. Only considered if RABBITMQ_SSL is true. |

### `mongodb.env`

MongoDB connection settings:

| Variable name                          | Default        | Description |
| -------------------------------------- | -------------- | ----------- |
| MONGODB_HOST                           | simces_mongodb | The host name for MongoDB. Can also be Docker container name for a container in the same Docker network as the Platform Manager. |
| MONGODB_PORT                           | 27017          | The port number for MongoDB. |
| MONGODB_USERNAME                       |                | The username for MongoDB. If this is empty, no access control is used. |
| MONGODB_PASSWORD                       |                | The password for MongoDB. |
| MONGODB_ADMIN                          | true           | Whether the account given in MONGODB_USERNAME has root access or not. Ignored if MONGODB_USERNAME is empty. (true/false) |
| MONGODB_TLS                            | false          | Whether the connection to MongoDB is secured or not (true/false) |
| MONGODB_TLS_ALLOW_INVALID_CERTIFICATES | false          | Whether to allow invalid security certificates. Ignored if MONGO_TLS is false. (true/false) |
