# Configuring platform settings

There are two collections of settings:

- [General platform level settings](#general-platform-level-settings) that affect the background components like Log Reader and require running the installation script to take an effect
- [Simulation run specific settings](#simulation-run-specific-settings) that affect the future simulation runs and can be changed without running the installation script

## General platform level settings

The default settings for the platform are fine for local testing.

If there is a need to connect to remote resources like a remote RabbitMQ message bus so that externally managed components that have not been locally deployed can participate in the simulations, some platform level settings must be modified.

There are 6 different files that define the settings for the core platform. They are all listed in the table below with the details given in the subsections. Only the the parameters that might require changing are described here. The default files contain comments for all settings.

For any changes to these files to have an effect, the platform core setup script must be run, see the step 6 at page [Installation](core_install.md#installing-the-platform-and-the-core-components).

| Configuration file                         | Description |
| ------------------------------------------ | ----------- |
| [background/env/components_mongodb.env](#backgroundenvcomponents_mongodbenv) | The MongoDB connection settings used by [Log Reader](core_log-api.md) and the instance of [Log Writer](core_logwriter.md) that is listening to the [management exchange](core_exchange-mgmt.md). |
| [background/env/components_logwriter.env](#backgroundenvcomponents_logwriterenv) | Additional settings for the instance of [Log Writer](core_logwriter.md) that is listening to the [management exchange](core_exchange-mgmt.md). Includes the RabbitMQ connection settings, logging level settings and other Log Writer specific settings. |
| [background/env/rabbitmq.env](#backgroundenvrabbitmqenv) | Settings for the locally deployed RabbitMQ message bus instance. |
| [background/env/mongodb.env](#backgroundenvmongodbenv) | Settings for the locally deployed MongoDB database instance. |
| [background/env/mongo_express.env](#backgroundenvmongo_expressenv) | Settings for the locally deployed Mongo Express instance that can be used to access the MongoDB and the messages from the simulations directly. |
| [background/docker-compose-background.yml](#backgrounddocker-compose-backgroundyml) | The Docker Compose file that is used to deploy all the core platform components. The locally used ports can be changed by modifying this file. |

### background/env/components_mongodb.env

MongoDB connection settings for [Log Reader](core_log-api.md) and the instance of of [Log Writer](core_logwriter.md) that is listening to the [management exchange](core_exchange-mgmt.md):

| Variable name                          | Default        | Description |
| -------------------------------------- | -------------- | ----------- |
| MONGODB_HOST                           | simces_mongodb | The host name for MongoDB. Can also be Docker container name for a container in the same Docker network as the Platform Manager. |
| MONGODB_PORT                           | 27017          | The port number for MongoDB. |
| MONGODB_USERNAME                       |                | The username for MongoDB. If this is empty, no access control is assumed to be in use. |
| MONGODB_PASSWORD                       |                | The password for MongoDB. |
| MONGODB_ADMIN                          | true           | Whether the account given in MONGODB_USERNAME has root access or not. Ignored if MONGODB_USERNAME is empty. (true/false) |
| MONGODB_TLS                            | false          | Whether the connection to MongoDB is secured or not (true/false) |
| MONGODB_TLS_ALLOW_INVALID_CERTIFICATES | false          | Whether to allow invalid security certificates. Ignored if MONGO_TLS is false. (true/false) |

The default file can be found at: [background/env/components_mongodb.env](https://github.com/simcesplatform/Platform-Manager/blob/master/background/env/components_mongodb.env)

### background/env/components_logwriter.env

Settings for the instance of [Log Writer](core_logwriter.md) that is listening to the [management exchange](core_exchange-mgmt.md). Includes the RabbitMQ connection settings, logging level settings and other Log Writer specific settings.

| Variable name        | Default         | Description |
| -------------------- | --------------- | ----------- |
| SIMULATION_LOG_LEVEL | 20              | The logging level for which the logging messages are included in the output of the components. 30 to include only warnings and errors, 20 to include also info messages, and 10 to include the debug messages as well. |
| RABBITMQ_HOST        | simces_rabbitmq | The host name for the RabbitMQ message bus. Can also be Docker container name for a container in the same Docker network as the Platform Manager. |
| RABBITMQ_PORT        | 5672            | The port number for the RabbitMQ message bus. |
| RABBITMQ_LOGIN       |                 | The username for the RabbitMQ message bus. If this is empty, the default username, `guest`. is used. |
| RABBITMQ_PASSWORD    |                 | The password for the RabbitMQ message bus. If this is empty, the default password, `guest`. is used. |
| RABBITMQ_SSL         | false           | Whether the connection to the RabbitMQ message bus is secured or not (true/false) |
| RABBITMQ_SSL_VERSION | PROTOCOL_TLS    | The security protocol used in the RabbitMQ message bus connection. Only considered if RABBITMQ_SSL is true. |

The default file can be found at: [background/env/components_logwriter.env](https://github.com/simcesplatform/Platform-Manager/blob/master/background/env/components_logwriter.env)

### background/env/rabbitmq.env

Settings for the locally deployed RabbitMQ message bus instance.

| Variable name         | Default | Description                                  |
| --------------------- | ------- | -------------------------------------------- |
| RABBITMQ_DEFAULT_USER |         | The username for the local RabbitMQ instance. If this is empty, the default username, `guest`. is used. |
| RABBITMQ_DEFAULT_PASS |         | The password for the local RabbitMQ instance. If this is empty, the default password, `guest`. is used. |

The default file can be found at: [background/env/rabbitmq.env](https://github.com/simcesplatform/Platform-Manager/blob/master/background/env/rabbitmq.env)

### background/env/mongodb.env

| Variable name              | Default | Description |
| -------------------------- | ------- | ----------- |
| MONGO_INITDB_ROOT_USERNAME |         | The username for root access to the local MongoDB instance. If this is empty, no access control is used. |
| MONGO_INITDB_ROOT_PASSWORD |         | The password for root access to the local MongoDB instance |

The default file can be found at: [background/env/mongodb.env](https://github.com/simcesplatform/Platform-Manager/blob/master/background/env/mongodb.env)

### background/env/mongo_express.env

The settings for Mongo Express.

| Variable name                   | Default        | Description |
| ------------------------------- | -------------- | ----------- |
| ME_CONFIG_MONGODB_SERVER        | simces_mongodb | The host name for MongoDB. Can also be Docker container name for a container in the same Docker network as the Platform Manager. |
| ME_CONFIG_MONGODB_PORT          | 27017          | The port number for MongoDB. |
| ME_CONFIG_MONGODB_ENABLE_ADMIN  | true           | Whether admin access is allowed though the Mongo Express interface (true/false) |
| ME_CONFIG_MONGODB_ADMINUSERNAME |                | The admin username for MongoDB. If this is empty and ME_CONFIG_MONGODB_ENABLE_ADMIN is true, no access control is assumed to be in use. |
| ME_CONFIG_MONGODB_ADMINPASSWORD |                | The admin password for MongoDB. |
| ME_CONFIG_MONGODB_AUTH_DATABASE | logs           | if ME_CONFIG_MONGODB_ENABLE_ADMIN is false, the database that is used to authenticate the MongoDB user |
| ME_CONFIG_MONGODB_AUTH_USERNAME |                | The username for non-admin user in MongoDB. |
| ME_CONFIG_MONGODB_AUTH_PASSWORD |                | The password for non-admin user in MongoDB. |
| ME_CONFIG_MONGODB_SSL           | false          | Whether the connection to MongoDB is secured or not (true/false) |
| ME_CONFIG_BASICAUTH_USERNAME    |                | The username asked by the Mongo Express web interface |
| ME_CONFIG_BASICAUTH_PASSWORD    |                | The password asked by the Mongo Express web interface |

The default file can be found at: [background/env/mongo_express.env](https://github.com/simcesplatform/Platform-Manager/blob/master/background/env/mongo_express.env)

### background/docker-compose-background.yml

Some local ports are used by the platform. The default ports are listed in the table below:

| Component     | Default port(s) |
| ------------- | --------------- |
| Log Reader    | 8080            |
| Mongo Express | 8081            |
| RabbitMQ      | 5672 and 15672  |

The used port numbers can be changed by modifying the Docker Compose file by changing the number in the relevant section. For example to change the locally deployed Log Reader to use port 8555 instead of the default 8080, the Log Reader section of the file should be changed to the following:

```yaml
log_reader:
    image: ghcr.io/simcesplatform/logreader
    container_name: simces_log_reader
    restart: always
    depends_on:
        - mongodb
    env_file:
        - env/components_mongodb.env
    environment:
        - MONGODB_APPNAME=log_writer
    ports:
        - 8555:8080  # here a change from 8080:8080 to 8555:8080
    networks:
        - mongodb_network
```

The default file can be found at: [background/docker-compose-background.yml](https://github.com/simcesplatform/Platform-Manager/blob/master/background/docker-compose-background.yml)

## Simulation run specific settings

The simulation configuration YAML file is the main configuration for each simulation run. However, for the communication to work some environment settings must be configured properly.

The environment variables are passed to [Platform Manager](core_platformmanager.md) when a new simulation run is started. They are given in 3 environment variable files: [`common.env`](#commonenv) (for some common settings), [`rabbitmq.env`](#rabbitmqenv) (for the connection details for RabbitMQ) and [`mongodb.env`](#mongodbenv) (for connection details for MongoDB). All of the common and RabbitMQ parameters are passed to the platform managed components when a new simulation run is started. In addition, the simulation run specific Log Writer instance also receives the MongoDB settings.

Some of the parameters are described here. For the parameters not described here there are descriptive comments in the files themselves.

### common.env

Common settings for all simulation components:

| Variable name        | Default | Description |
| -------------------- | ------- | ----------- |
| SIMULATION_LOG_LEVEL | 20      | The logging level for which the logging messages are included in the output of the components. 30 to include only warnings and errors, 20 to include also info messages, and 10 to include the debug messages as well. |

The default file can be found at: [common.env](https://github.com/simcesplatform/Platform-Manager/blob/master/common.env)

### rabbitmq.env

RabbitMQ connection settings for a new simulation run:

| Variable name        | Default         | Description |
| -------------------- | --------------- | ----------- |
| RABBITMQ_HOST        | simces_rabbitmq | The host name for the RabbitMQ message bus. Can also be Docker container name for a container in the same Docker network as the Platform Manager. |
| RABBITMQ_PORT        | 5672            | The port number for the RabbitMQ message bus. |
| RABBITMQ_LOGIN       |                 | The username for the RabbitMQ message bus. If this is empty, the default username, `guest`. is used. |
| RABBITMQ_PASSWORD    |                 | The password for the RabbitMQ message bus. If this is empty, the default password, `guest`. is used. |
| RABBITMQ_SSL         | false           | Whether the connection to the RabbitMQ message bus is secured or not (true/false) |
| RABBITMQ_SSL_VERSION | PROTOCOL_TLS    | The security protocol used in the RabbitMQ message bus connection. Only considered if RABBITMQ_SSL is true. |

The default file can be found at: [rabbitmq.env](https://github.com/simcesplatform/Platform-Manager/blob/master/rabbitmq.env)

### mongodb.env

MongoDB connection settings for a new simulation run:

| Variable name                          | Default        | Description |
| -------------------------------------- | -------------- | ----------- |
| MONGODB_HOST                           | simces_mongodb | The host name for MongoDB. Can also be Docker container name for a container in the same Docker network as the Platform Manager. |
| MONGODB_PORT                           | 27017          | The port number for MongoDB. |
| MONGODB_USERNAME                       |                | The username for MongoDB. If this is empty, no access control is assumed to be in use. |
| MONGODB_PASSWORD                       |                | The password for MongoDB. |
| MONGODB_ADMIN                          | true           | Whether the account given in MONGODB_USERNAME has root access or not. Ignored if MONGODB_USERNAME is empty. (true/false) |
| MONGODB_TLS                            | false          | Whether the connection to MongoDB is secured or not (true/false) |
| MONGODB_TLS_ALLOW_INVALID_CERTIFICATES | false          | Whether to allow invalid security certificates. Ignored if MONGO_TLS is false. (true/false) |

The default file can be found at: [mongodb.env](https://github.com/simcesplatform/Platform-Manager/blob/master/mongodb.env)
