# Dummy Component

## Description

Dummy component is a simulation component that can be used to test the simulation platform installation. It can also be used to add delays to the execution of the simulation run if there is a desire to follow the progress of the simulation during the simulation run.

## Functionalities

- Listens to the message bus and catches all messages for the topics `SimState` and `Epoch` in the given exchange.
- Offers parameters that can be used to adjust waiting times before the component sends [Status](core_msg-status.md) message.
- Offers parameters that can be used to set the probabilities that the component fails to react to a new [Epoch](core_msg-epoch.md) message or fails to send a [Status](core_msg-status.md) message.
- Offers parameter that can be used to set the probability that the component sends an [Error](core_msg-status.md#error-message) message instead of [Ready](core_msg-status.md#ready-message) message.
- Sends a randomly generated result message to a topic `Result` before sending a [Status](core_msg-status.md) message for each epoch.

## Technical details

- Dummy component is written in Python (3.7.9) and the source code is part of [Simulation Manager](core_simulationmanager.md) repository. The code related to the dummy component specifically is found at folder: [https://github.com/simcesplatform/Simulation-Manager/tree/master/dummy](https://github.com/simcesplatform/Simulation-Manager/tree/master/dummy)
- Dummy component connects to RabbitMQ message bus. Both local and remote message bus servers are supported.

## Workflow

In the following `rand(a, b)` is a random number in the interval [a, b].

- 1\. Receive the message bus connection parameters and dummy component related parameters upon startup.
- 2\. Open the connection to the message bus and start a message listener for topics SimState and Epoch.
- 3\. After receiving simulation start message respond with status ready message.
- 4\. For each incoming epoch message received from the message bus:
    - 4\.1\. If `rand(0, 1) < RECEIVE_MISS_CHANCE`, ignore the epoch message. Otherwise, move to 4.2.
    - 4\.2\. If `rand(0, 1) < ERROR_CHANCE`, send an error message. Otherwise, move to 4.3.
    - 4\.3\. Wait for `rand(MIN_SLEEP_TIME, MAX_SLEEP_TIME)` seconds.
    - 4\.4\. Create and send a randomly created result message to topic Result.
    - 4\.5\. If `rand(0, 1) < SEND_MISS_CHANCE`, skip sending a status ready message. Otherwise, move to 4.6.
    - 4\.6\. Send a status ready message for the epoch.
        - If `rand(0, 1) < WARNING_CHANCE`, include a warning in the status message.
- 5\. Close the component after receiving a simulation stopped message.

## Input parameters

| Parameter name | Datatype | Corresponding [Start message](core_msg-start.md#dummy-component-block) attribute | Default | Description |
| --- | --- | --- | --- | --- |
| MIN_SLEEP_TIME      | Float | MinSleepTime      | 2.0  | Minimum waiting time in seconds before sending Status message. |
| MAX_SLEEP_TIME      | Float | MaxSleepTime      | 15.0 | Maximum waiting time in seconds before sending Status message. |
| WARNING_CHANCE      | Float | WarningChance     | 0.0  | Probability (0-1) that a warning is included in Status message. |
| SEND_MISS_CHANCE    | Float | SendMissChance    | 0.0  | Probability (0-1) that sending Status message is skipped after processing the epoch. |
| RECEIVE_MISS_CHANCE | Float | ReceiveMissChance | 0.0  | Probability (0-1) that received Epoch message is ignored. |
| ERROR_CHANCE        | Float | ErrorChance       | 0.0  | Probability (0-1) that Error message is sent after receiving Epoch message. |
