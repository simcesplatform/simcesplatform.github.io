# NetworkState.Current

## JSON structure

```nohighlight
{
  (Fields of AbstractMessage and AbstractResult must appear too!)

  "MagnitudeSendingEnd" : { "Value": 0.1, "UnitOfMeasure": "A" },
  "MagnitudeReceivingEnd" : { "Value": 0.2, "UnitOfMeasure": "A" },
  "AngleSendingEnd" : { "Value": 10.0, "UnitOfMeasure": "deg" },
  "AngleReceivingEnd" : { "Value": 20.0, "UnitOfMeasure": "deg" },
  "DeviceId" : "xyz-3",
  "Phase" : 3
}
```


## Fields and multiplicity

| Field | Type | Multiplicity | Explanation |
|-|-|-|-|
| (All fields from [AbstractResult](core_msg-abstractresult.md) and [AbstractMessage](core_msg-abstractmessage.md)) | | | Fields from the "abstract base class" |
| MagnitudeSendingEnd | Quantity block | 1 (REQUIRED) | Always in "A". Sign is towards the branch. Sending end. |
| MagnitudeReceivingEnd | Quantity block | 1 (REQUIRED) | Always in "A". Sign is towards the branch. Receiving end. |
| AngleSendingEnd | Quantity block | 1 (REQUIRED) | Always in degrees ("deg" as specified in UCUM); sending end |
| AngleReceivingEnd | Quantity block | 1 (REQUIRED) | Always in degrees ("deg" as specified in UCUM); receiving end |
| DeviceId | String | 1 (REQUIRED) | Must correspond to an entry in network model branches |
| Phase | Integer | 1 (REQUIRED) | Allowed values 1,2 and 3 |
