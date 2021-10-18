# LogWriter

## Description

LogWriter is a simulation component and it is part of the [Logging System](core_logging-system.md).
It is used to log all the messages in a simulation run and thus an instance of LogWriter is part of every simulation run.

## Functionalities

- Listen to the message bus and catch all messages in the given exchange.
- Keep track of the running simulations using the given exchange and collect some metadata for each simulation.
    - Collected metadata include at least the names of the participating processes and the timestamps for the first and the last message send during the simulation.
- Separate valid simulation platform messages from invalid messages that are not in JSON format or are missing one of the base attributes.
- Write all the simulation messages to a database.
    - Received valid messages are stored in simulation-specific collections.
    - Received invalid messages are stored in separate simulation-specific collections for debugging purposes.
- Writes the gathered simulation metadata to a database.

## Technical details

- LogWriter is written in Python (3.7.9) and the code repository is found at: [https://github.com/simcesplatform/logwriter](https://github.com/simcesplatform/logwriter)
- LogWriter connects to RabbitMQ message bus. Both local and remote message bus servers are supported.
- The used database is MongoDB with version 4.2.7 used during development.
- LogWriter has support for HTTPS connections for the database.
- LogWriter requires either admin or write access to the used database.
- One LogWriter instance will only listen to messages from one RabbitMQ exchange.

## Workflow

1. Receive the message bus and database connection parameters upon startup.
2. Open the connection to the message bus and start a message listener for all topics in the given exchange.
3. Open the database connection.
4. For each message received from the message bus:
    - Write the message to the database.
        - Each simulation will have their own collection of messages in the database. The collection is created when the first message for the simulation is received.
        - The collection name for the simulation messages is "simulation_<simulation_id>".
    - Possibly update the simulation metadata.
        - The metadata documents are only updated after SimState or Epoch messages.
        - Each simulation will have one metadata document in the database.
        - The metadata documents are all collected under the same collection (called "simulations") in the database.
    - If the message was SimState message indicating that the simulation is stopped, stop the LogWriter instance.

Note, that originally LogWriter was made to support multiple simulations but due to specification changes, each simulation run in the SimCES platform will use a separate exchange. Thus, a separate instance of LogWriter is needed for each simulation run. Due to this LogWriter will stop once it has received the message indicating that the simulation has stopped, i.e. SimState message with the value "stopped", and all the received messages have been written to the database.

When writing the messages to the database LogWriter uses a buffer to reduce the number of times the database is accessed. All received messages are first added to the buffer and they are only written to the database once the buffer is cleared.

The message buffer will be cleared:

- when the latest received message was either a SimState or an Epoch message.
- when the maximum number of messages, by default 20, in the buffer is reached. The maximum number of messages in the buffer is configurable by the input parameter `MessageBufferMaxDocumentCount`.
- when at least 10 seconds have gone after a message was added to the buffer without the buffer having been cleared. The number of seconds is configurable by the input parameter `MessageBufferMaxInterval`.

## Input parameters

| Parameter name                | Datatype      | Example | Default value |
| ----------------------------- | ------------- | ------- | ------------- |
| MessageBufferMaxDocumentCount | Integer (> 0) | 10      | 20            |
| MessageBufferMaxInterval      | Float (> 0)   | 5.0     | 10.0          |

## Accessing the messages

The simulation messages and data contained within them can be accessed using the LogReader part of the [Logging System](core_logging-system.md) with the [Read/Export API](core_log-api.md).

To speed up the queries fetching data from the simulation messages stored in the database, LogWriter adds some indexes to both the metadata document collection as well as to the simulation specific message collections.
