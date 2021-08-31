# ResourceForecastState.Dispatch

## JSON structure

```nohighlight
{

  // *** Messages MUST be encoded in UTF-8! ***

  // --- Fields of AbstractMessage begin ---
  "Type" : "set-message-type-here",
  "Timestamp" : "2020-06-03T04:04:21.045Z",
  "SimulationId" : "2020-06-03T04:01:52.345Z",
  "SourceProcessId" : "set-process-id-here",
  "MessageId" : "set-message-id-here",
  // --- Fields of AbstractMessage end ---

  // --- Fields of AbstractResult begin ---
  "EpochNumber" : 14,
  "IterationStatus" : "final", // leave out when not iterating
  "LastUpdatedInEpoch" : 14, // OPTIONAL field
  "TriggeringMessageIds" : // TriggeringMessageIds are irrelevant in Epoch messages?
  [
    "epoch-14",
    "someprocess-14",
    "anotherprocess-14"
  ],
  "Warnings" : // If warnings precede an error, it can be beneficial to provide them as well in the Error message
  [
    "warning.convergence"
  ],
  // --- Fields of AbstractResult end ---

"Dispatch" : {
  "ResourceA" : {
    "TimeIndex" :
    [
      "2020-06-25T00:00:00Z",
      "2020-06-25T01:00:00Z"
    ],
    "Series" : {
      "RealPower" : {
        "UnitOfMeasure" : "kW",
        "Values" :
        [
          0.2,
          0.27
        ]
        }
      }
    },
  "ResourceB" : {
    "TimeIndex" :
    [
      "2020-06-25T00:00:00Z",
      "2020-06-25T01:00:00Z
    ],
    "Series" : {
      "RealPower" : {
        "UnitOfMeasure" : "kW",
        "Values" :
        [
          0.27,
          0.2
        ]
        }
      }
    }
  }
}
```

## Fields and multiplicity

| Field | Type | Multiplicity | Explanation |
| --- | --- | --- | --- |
| All fields from [AbstractResult](core_msg-abstractresult.md) | | | Fields from "abstract base class" |
| Dispatch | Dictionary; key is string, value is [Time series block](core_block-time-series.md) | 1 (REQUIRED) | The key is the process identifier, value is a time series (power) |

