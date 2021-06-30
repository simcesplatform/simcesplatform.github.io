# Start message

This message notifies components about the startup of a simulation run, delivering simulation-run-specific parameters.

## JSON structure

```nohighlight
{
  "Type" : "Start",
  "Timestamp" : "2020-08-31T04:04:21.045Z",
  "SimulationId" : "set-simulation-id-here",
  "SimulationSpecificExchange" : "set-exchange-name-here",
  "SimulationName" : "set-name-here",
  "SimulationDescription" : "set-description-here",
  "ProcessParameters" :
  {
   
    "SimulationManager": // There is only one Simulation Manager for each simulation run
    {
      "ManagerName": "Manager",
      "InitialStartTime": "2020-06-28T00:00:00.000Z",
      "EpochLength": 3600,
      "MaxEpochCount": 24,
      "Components": [
        "Load1",
        "Load2",
        "Generator",
        "GridA"
      ],
      "EpochTimerInterval": 120.0,  // optional
      "MaxEpochResendCount": 5      // optional
    },

    "LogWriter":  // There is only one Log Writer for each simulation run
    {
      "MessageBufferMaxDocumentCount": 10,  // optional, default is 20
      "MessageBufferMaxInterval": 5.0       // optional, default is 10.0
    },

    "Dummy":  // Include this if relevant
    {
      "DummyA":  // The identifier for the dummy component
      {
        "MinSleepTime": 1.0,       // optional, default is 2.0
        "MaxSleepTime": 2.5,       // optional, default is 15.0
        "WarningChance": 0.0,      // optional, default is 0.0
        "SendMissChance": 0.0,     // optional, default is 0.0
        "ReceiveMissChance": 0.0,  // optional, default is 0.0
        "ErrorChance": 0.0,        // optional, default is 0.0
      }
    },

    "StaticTimeSeriesResource":  // Example parameters for a component
    {
      "LoadA":  // The identifier for the resource
      {
        "ResourceType": "Load",
        "ResourceStateFile": "load.csv"
      },
      "GeneratorA":
      {
        "ResourceType": "Generator",
        "ResourceStateFile": "generator.csv"
      }
    }

  }
}
```


## Fields and multiplicity

| Field | Type | Multiplicity | Explanation |
|-|-|-|-|
| Type | String | 1 (REQUIRED) | The name of the message type. For Start message this MUST be equal to "Start". |
| Timestamp | ISO 8601; see (1) | 1 (REQUIRED) | The time when the message was generated |
| SimulationId | String | 1 (REQUIRED) | The unique identifier of the simulation run, first published in this message (i.e., specified by the sender). |
| SimulationSpecificExchange | String | 1 (REQUIRED) | The name of the simulation-specific Exchange that will be the channel to communicate simulation-run-specific messages |
| SimulationName | String | 0..1 (OPTIONAL) | A human-readable name to help in identifying the simulation run |
| SimulationDescription | String | 0..1 (OPTIONAL) | A human-readable description to help in identifying the simulation run |
| ProcessParameters | Object that contains other objects | 0..1 (OPTIONAL) | Specifies any process-specific parameters; see (2) |

- (1)
    - Date and time included
    - Accuracy 1 ms
    - UTC zone
- (2): REQUIRED if there are any process-specific parameters to deliver. For each object, the name is a string that matches to the identifier of the process referred to. The value is a structure defined in "process parameter blocks" section.


## Process parameter blocks

This section explains the process parameter blocks used by platform core. This excludes any domain-specific blocks.

Note: the Platform Manager passes these parameters directly to the Simulation Manager instance through environmental variables. They are included as part of the Start message to make the used parameters visible to the other components as well as to the Logging System.

### Simulation Manager block

| Field | Type | Multiplicity | Explanation |
|-|-|-|-|
| ManagerName | String | 1 (REQUIRED) | The identifier, i.e. the SourceProcessId, for the Simulation Manager instance. |
| InitialStartTime | ISO 8601; see (a) | 1 (REQUIRED) | The start time for the first epoch as ISO 8601 formatted datetime string. |
| EpochLength | Integer (> 0) | 1 (REQUIRED) | The epoch length in seconds. |
| MaxEpochCount | Integer (> 0) | 1 (REQUIRED) | The maximum number of epochs in the simulation run. |
| Components | Array of strings | 1..* (at least one REQUIRED) | An array of the names of the components participating in the simulation. The names MUST correspond to the identifiers (i.e. the SourceProcessId) used by the components. |
| EpochTimerInterval | Float | 0..1 (OPTIONAL) | The time interval in seconds until Simulation Manager resends an Epoch message if some component has not responded with Status message. The default value is 120 seconds. |
| MaxEpochResendCount | Integer | 0..1 (OPTIONAL) | The maximum number of Epoch message resends Simulation Manager can try. The default value is 5 resends. |

- (a)
    - Date and time included
    - Accuracy 1 ms
    - Time zone information MUST be included

### Log Writer block

| Field | Type | Multiplicity | Explanation |
|-|-|-|-|
| MessageBufferMaxDocumentCount	 | Integer (> 0) | 1 (OPTIONAL) | The maximum number of messages the buffer in Log Writer can hold before the messages are written to the database and the buffer is cleared. The default value is 20. |
| MessageBufferMaxInterval | Float | 1 (OPTIONAL) | The maximum time interval in seconds before the message buffer in Log Writer is cleared and the messages written to the database. The default value is 10 seconds. |

### Dummy Component block

| Field | Type | Multiplicity | Explanation |
|-|-|-|-|
| MinSleepTime | Float | 1 (OPTIONAL) | The minimum time in seconds the Dummy component waits after receiving an Epoch message before sending the Status message. The default value is 2 seconds. |
| MaxSleepTime | Float | 1 (OPTIONAL) | The maximum time in seconds the Dummy component waits after receiving an Epoch message before sending the Status message. The default value is 15 seconds. |
| WarningChance | Float | 1 (OPTIONAL) | The probability that a warning is included in the Status message sent to the Simulation Manager. 1 means that a warning is always included and 0 means that a warning is never included. The default value is 0. |
| SendMissChance | Float | 1 (OPTIONAL) | The probability that the Dummy component does not send the Status message after processing the epoch to the simulation manager. 1 means that the Status messages are never sent and 0 means that they are always sent. The default value is 0. |
| ReceiveMissChance | Float | 1 (OPTIONAL) | The probability that the Dummy component ignores a received Epoch message. 1 means that the Epoch messages are always ignored and 0 means that they are never ignored. The default value is 0. |
| ErrorChance | Float | 1 (OPTIONAL) | The probability that the Dummy component sends an error message after a received Epoch message. 1 means that an error message is always sent and 0 means that an error message is never included. The default value is 0. |
