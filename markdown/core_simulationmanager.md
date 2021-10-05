# Simulation Manager

Simulation Manager is the main component in any simulation run on the SimCES simulation platform.
It handles starting and stopping the simulation run as well as starting a new epoch in the simulation run once all participating components have reported that they are ready for a new epoch.

## Functionalities

The Simulation Manager has two responsibilities: the Manager part is responsible for starting and stopping the simulation run and the Omega part is responsible for starting new epochs within the simulation run.

- Signals other processes about starting and stopping a simulation round (Manager part). This signaling occurs via [SimState message](core_msg-simstate.md).
    - Simulation is ended after reaching the maximum number of epochs or by the simulation manager receiving an error message (see [Status message](core_msg-status.md)).
- Manages time within a simulation by sending epoch messages (Omega part) (see [Time and synchronization](core_time.md) and [Epoch message](core_msg-epoch.md)).
    - New epoch messages are only sent once all participating components have finished calculations for the epoch. The components signal the Simulation manager that they are finished for the ongoing epoch by using "ready" messages (see [Status message](core_msg-status.md)).

## Technical details

- Simulation Manager is written in Python (3.7.9) and the code repository is found at: [https://github.com/simcesplatform/simulation-manager](https://github.com/simcesplatform/simulation-manager)
- Simulation Manager connects to RabbitMQ message bus. Both local and remote message bus servers are supported.

## Workflow

1. Simulation is started.
2. Simulation Manager receives a list of the component names as a parameter during the startup.
3. Simulation Manager sends [SimState message](core_msg-simstate.md) "running" to start the simulation.
    - The SimState message is sent after a shortly after the Simulation Manager has started. The delay is hardcoded to 10 seconds.
4. The other simulation components respond with a [ready message](core_msg-status.md) to indicate that they are available. The used epoch number for this starting status message is 0.
5. Once all the simulation components have responded with a "ready" message, Simulation Manager sends an [Epoch message](core_msg-epoch.md) for a new epoch. The epoch number for the first epoch is 1.
    - If Simulation Manager does not receive a "ready" message from all the components within a certain time limit, Simulation Manager resends the Epoch message for the running epoch.
        - The resend can be turned off by a parameter (by setting `MaxEpochResendCount` to 0) and the time limit can also be changed by a parameter (with `EpochTimerInterval`).
            - The default value for the resend timer is 2 minutes with a maximum of 5 resends. The time limit is increased with each resend, i.e. the first resend Epoch message is sent 2 minutes after the original message, the second resend Epoch message is sent 2+2=4 minutes after the first resend, the third resend Epoch message is sent 6 minutes after the second resend, ...
            - If the maximum number of resends is reached, SimulationManager will stop the simulation by sending [SimState message](core_msg-simstate.md) "stopped".
        - These resends require that the simulation components recognize that a received Epoch message might not be for a new epoch.
    - The Epoch messages sent by Simulation Manager contain a list of triggering message ids that can be used by the simulation components.
        - In the case of an Epoch message for a new epoch, the triggering message list will contain "ready" [Status message](core_msg-status.md) ids for the previous epoch in the order the messages were received.
            - In the case of a resend Epoch message, the triggering message list will contain those "ready" message ids that have been received by Simulation Manager for the running epoch.
6. The simulation components receive the epoch message and do whatever they need to do within the epoch and send a "ready" message when they are finished with the epoch 1.
7. Repeat steps 5 and 6 for with a growing epoch number in each epoch. I.e. epoch numbers being 2, 3, 4, ...
8. Once maximum number epochs are finished or Simulation Manager receives an [Error message](core_msg-status.md), Simulation Manager sends a [SimState message](core_msg-simstate.md) "stopped".
9. Simulation is stopped.
    - Simulation Manager closes itself after sending the last [SimState message](core_msg-simstate.md).

## Environment variables

When using the SimCES platform, the [Platform Manager](core_platformmanager.md) will sends all the requires variables to the Simulation Manager according to the configuration of the simulation run.

| Variable name                   | Datatype          | Corresponding [Start message](core_msg-start.md) attribute | Description |
| ------------------------------- | ----------------- | ------------------------------------- | ----------- |
| SIMULATION_ID                   | ISO 8601; see (a) | SimulationId            | The id for the simulation run. It is expected to correspond to the current time given in UTC time zone. |
| SIMULATION_MANAGER_NAME         | String            | ManagerName (b)         | The identifier, i.e. the SourceProcessId, for the Simulation Manager instance. |
| SIMULATION_COMPONENTS           | String            | Components (b)          | A comma-separated string of the names of the components participating in the simulation. The names MUST correspond to the identifiers (i.e. the SourceProcessId) used by the components. |
| SIMULATION_NAME                 | String            | SimulationName          | The name of the simulation run. |
| SIMULATION_DESCRIPTION          | String            | SimulationDescription   | The description of the simulation run. |
| SIMULATION_EPOCH_LENGTH         | Integer (> 0)     | EpochLength (b)         | The epoch length in seconds. |
| SIMULATION_INITIAL_START_TIME   | ISO 8601; see (a) | InitialStartTime (b)    | The start time for the first epoch as ISO 8601 formatted datetime string. |
| SIMULATION_MAX_EPOCHS           | Integer (> 0)     | MaxEpochCount (b)       | The maximum number of epochs in the simulation run. |
| SIMULATION_EPOCH_TIMER_INTERVAL | Float (> 0)       | EpochTimerInterval (b)  | The time interval in seconds until Simulation Manager resends an Epoch message if some component has not responded with Status message. |
| SIMULATION_MAX_EPOCH_RESENDS    | Integer (>= 0)    | MaxEpochResendCount (b) | The maximum number of Epoch message resends Simulation Manager can try for each epoch. |

- (a)
    - Date and time included
    - Accuracy 1 ms
    - Time zone information MUST be included
- (b)
    - In the Simulation Manager block of the Start message

In addition to those variables given above, Simulation Manager also needs the environment variables that relate to connecting to the RabbitMQ message bus.
