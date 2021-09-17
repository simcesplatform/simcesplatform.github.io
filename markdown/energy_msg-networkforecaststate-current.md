# NetworkForecastState.Current

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
"Forecast" : {
   "TimeIndex" :
   [
     "2020-06-25T00:00:00Z",
     "2020-06-25T01:00:00Z",
     "2020-06-25T02:00:00Z",
     "2020-06-25T03:00:00Z"
   ],
  "Series" :
   {
     "MagnitudeSendingEnd" :
       {
         "UnitOfMeasure" : "A",
         "Values" :
         [
           0.2,
           0.27,
           0.15,
           0.21
         ]
       }
     "MagnitudeReceivingEnd" :
      {
       "UnitOfMeasure" : "A",
       "Values" :
       [
          2,
          27,
          15,
          1
       ]
      }
     "AngleSendingEnd" :
     {
      "UnitOfMeasure" : "degree",
      "Values" :
      [
        0.2,
        0.27,
        0.15,
        0.21
      ]
     }
    "AngleReceivingEnd" :
    {
     "UnitOfMeasure" : "degree",
     "Values" :
     [
       2,
       27,
       15,
       1
     ]
    }
   }
  }
  "DeviceId" : "XYZ",
  "Phase" : 1
}
```

## Fields and multiplicity

| Field | Type | Multiplicity | Explanation |
|-|-|-|-|
| (All fields from [AbstractResult](core_msg-abstractresult.md) and [AbstractMessage](core_msg-abstractmessage.md)) | | | Fields from the "abstract base class" |
| Forecast | [Time series block] (core_block-time-series.md) | **"MagnitudeSendingEnd"** (REQUIRED) always in "A". Sign is towards the branch. Sending End, **"MagnitudeReceivingEnd"** (REQUIRED) always in "A". Sign is towards the branch. Receiving End, **"AngleSendingEnd"**  (REQUIRED) always in degree. ("deg" as specified in UCUM). Sending End, **"AngleReceivingEnd"** (REQUIRED) always in degree. ("deg" as specified in UCUM). Receiving End | The time MUST be ordered from lowest to highest (i.e., ascending). |
| DeviceId | String | 1 (REQUIRED) | Must correspond to an entry in network model branches |
| Phase | Integer | 1 (REQUIRED) | Allowed values 1,2 and 3 |
