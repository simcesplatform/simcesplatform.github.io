# LFMMarketResult

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
"ActivationTime" : "2020-06-03T04:00:00.000Z",
"Duration" : { "Value": 60, "UnitOfMeasure": "Minute" },
"Direction" : "upregulation",
"RealPower" : {
   "TimeIndex" :
   [
     "2020-06-03T04:00:00.000Z",
     "2020-06-03T04:15:00.000Z",
     "2020-06-03T04:30:00.000Z",
     "2020-06-03T04:45:00.000Z"
   ],
  "Series" :
   {
     "Regulation" :
       {
         "UnitOfMeasure" : "kW",
         "Values" :
         [
           200,
           300,
           150,
           210
         ]
       }
     }
  }
"Price" :{ "Value": 50, "UnitOfMeasure": "EUR" },
"CustomerIds" : [ "CustomerIdA", "CustomerIdB" ],
"CongestionId" : "XYZ", 
"OfferId" : "Ele-2020-06-03T04:15:07.456Z",
"ResultCount" : 3
}
```

## Fields and multiplicity

| Field | Type | Multiplicity | Explanation |
| --- | --- | --- | --- |
| All fields from [AbstractResult](core_msg-abstractresult.md) | | | Fields from "abstract base class" |
| ActivationTime | ISO 8601, Date and time included, Accuracy 1 ms, UTC zone | 1 (REQUIRED) | The time when the flexibility requires to be activated |
| Duration | [Quantity block](core_block-quantity.md) | 1 (REQUIRED)| The duration when the flexibility requires to keep activated.  The value MUST equal to a positive-integer coefficient of 15 minutes (e.g., 15, 30, 45, 60, 75 etc) |
| Direction | String | 1 (REQUIRED)| Allowed values: "upregulation", "downregulation" |
| RealPower | [Time series block](core_block-time-series.md) | "Quantity" [Quantity block](core_block-quantity.md) (REQUIRED) always in "kW" | The value (always in kW) MUST:  equal to a positive-integer coefficient of BidResolution (i.e., simulation run parameter),  equal or larger than min bid size (i.e., simulation run parameter)|
| Price | [Quantity block](core_block-quantity.md) | 1 (REQUIRED) | |
| CongestionId | String | 1 (REQUIRED) | The value corresponds to the unique identifier of the congestion problem determined by DSO (PGO) |
| OfferId | String | 1 (REQUIRED) | The unique Id specific for the offer |
| ResultCount | Integer | 1 (REQUIRED) | Number of currently in-effect MarketResults |
| CongestionIds | Array of string | 1 (REQUIRED) | Lists the CustomerIds participating in the offer. |


