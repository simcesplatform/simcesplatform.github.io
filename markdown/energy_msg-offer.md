# Offer

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
"CongestionId" : "XYZ", 
"CustomerIds" : [ "CustomerIdA", "CustomerIdB" ], 
"OfferId" : "Elenia-2020-06-03T04:15:07.456Z",
"OfferCount" : 1
}
```

## Fields and multiplicity

| Field | Type | Multiplicity | Explanation |
| --- | --- | --- | --- |
| All fields from [AbstractResult](core_msg-abstractresult.md) | | | Fields from "abstract base class" |
| ActivationTime | ISO 8601, Date and time included, Accuracy 1 ms, UTC zone | 1 (REQUIRED), (OPTIONAL if OfferCount = 0) | The time when the flexibility requires to be activated |
| Duration | [Quantity block](core_msg-block-quantity.md) | 1 (REQUIRED), (OPTIONAL if OfferCount = 0) | The duration when the flexibility requires to keep activated. The value MUST equal to a positive-integer coefficient of 15 minutes (e.g., 15, 30, 45, 60, 75 etc) |
| Direction | String | 1 (REQUIRED), (OPTIONAL if OfferCount = 0) | Allowed values: "upregulation", "downregulation" |
| RealPower | [Time series block](core_block-time-series.md) | 1 (REQUIRED), (OPTIONAL if OfferCount = 0) | Offered values of regulation always in "kW". Series "Regulation" |
| Price | [Quantity block](core_msg-block-quantity.md) | 1 (REQUIRED), (OPTIONAL if OfferCount = 0) | |
| CongestionId | String | 1 (REQUIRED) | The value corresponds to the unique identifier of the congestion problem determined by DSO (PGO) |
| CustomerIds | Array of strings | 1 (REQUIRED) | Lists the CustomerIds participating in the offer. |
| OfferId | String | 1 (REQUIRED), (OPTIONAL if OfferCount = 0) | The unique Id specific for the offer |
| OfferCount | Integer | 1 (REQUIRED) | Total number of offers the provider is going to send in the running epoch related to this congestion area id |

