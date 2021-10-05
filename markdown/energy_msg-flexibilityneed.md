# FlexibilityNeed

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
"Duration" : { "Value": 30, "UnitOfMeasure": "Minute" },
"Direction" : "upregulation",
"RealPowerMin" :{ "Value": 100.0, "UnitOfMeasure": "kW" },
"RealPowerRequest" :{ "Value": 700.0, "UnitOfMeasure": "kW" },
"CustomerIds" : ["Ele10","Ele170"],
"CongestionId" : "XYZ"
"BidResolution" : { "Value": 10, "UnitOfMeasure": "kW" }
}
```

## Fields and multiplicity

| Field | Type | Multiplicity | Explanation |
| --- | --- | --- | --- |
| All fields from [AbstractResult](core_msg-abstractresult.md) | | | Fields from "abstract base class" |
| ActivationTime | ISO 8601, Date and time included, Accuracy 1 ms, UTC zone | 1 (REQUIRED), (OPTIONAL if OfferCount = 0) | The time when the flexibility requires to be activated |
| Duration | [Quantity block](core_block-quantity.md) | 1 (REQUIRED)| The duration when the flexibility requires to keep activated.  The value MUST equal to a positive-integer coefficient of 15 minutes (e.g., 15, 30, 45, 60, 75 etc) |
| Direction | String | 1 (REQUIRED)| Allowed values: "upregulation", "downregulation" |
| RealPowerMin | [Time series block](core_block-time-series.md) | 1 (REQUIRED) | The value (always in kW) MUST:  equal to a positive-integer coefficient of BidResolution (i.e., simulation run parameter),  equal or larger than min bid size (i.e., simulation run parameter)|
| RealPowerRequest | [Time series block](core_block-time-series.md) | 1 (REQUIRED) | The value (always in kW) MUST:  equal to a positive-integer coefficient of BidResolution (i.e., simulation run parameter),  equal or larger than min bid size (i.e., simulation run parameter)|
| CustomerIds | Array of strings | 1 (REQUIRED) | The value corresponds to all the CustomerIds within the congestion area.|
| CongestionId | String | 1 (REQUIRED) | The value corresponds to the unique identifier of the congestion problem determined by DSO (PGO) |
| BidResolution | [Quantity block](core_block-quantity.md) | 1 (OPTIONAL)| In kW, possible bid values between RealPowerMin and RealPowerRequest  .e.g., if RealPowerMin = 1 kW and RealPowerRequest = 10 kW and BidResolution = 1 kW, allowable bids are 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 kW |


