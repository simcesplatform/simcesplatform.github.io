# Start message

This message notifiers about the startup of a simulation run. It communicates simulation-run-specific parameters.

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

TODO: blocks
