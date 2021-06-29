# AbstractMessage message

This page specifies the common fields of all messages. This structure is not meaningful as a standalone message, so it is analogous to an abstract base class in object-oriented programming.


## JSON structure

```nohighlight
"Type" : "set-message-type-here",
"Timestamp" : "2020-06-03T04:04:21.045Z",
"SimulationId" : "2020-06-03T04:01:52.345Z",
"SourceProcessId" : "set-process-id-here",
"MessageId" : "set-message-id-here",
```


## Fields and multiplicity

| Field | Type | Multiplicity | Explanation |
|-|-|-|-|
| Type | String | 1 (REQUIRED) | Name of the message type. This field facilitates the processing of incoming messages and possibly debugging as well. |
| Timestamp | ISO 8601; see (1) | 1 (REQUIRED) | The time when the message was generated |
| SimulationId | String | 1 (REQUIRED) | The unique identifier of the simulation run |
| SourceProcessId | String | 1 (REQUIRED) | The name of the process that sent the message. This MUST be unique within the simulation run. |
| MessageId | String | 1 (REQUIRED) | The unique ID of the message within the simulation run. This is the name of the process + a running ID. |

- (1): Date and time MUST be included, accuracy MUST be 1 ms, time zone MUST be UTC
