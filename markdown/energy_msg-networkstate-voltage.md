# NetworkState.Voltage

## JSON structure

```nohighlight
{
  (Fields of AbstractMessage and AbstractResult must appear too!)

  ...
  "Magnitude" : {
    "Value" : 227.6,
    "UnitOfMeasure" : "kV"
  },
  "Angle" : {
    "Value" : 119.5,
    "UnitOfMeasure" : "deg"
  },
  "Bus" : "loadbus",
  "Node" : 2
}
```


## Fields and multiplicity

| Field | Type | Multiplicity | Explanation |
|-|-|-|-|
| (All fields from [AbstractResult](core_msg-abstractresult.md) and [AbstractMessage](core_msg-abstractmessage.md)) | | | Fields from the "abstract base class" |
| Magnitude | Quantity block | 1 (REQUIRED) | Always in "kV" |
| Angle | Quantity block | 1 (REQUIRED) | Always in degrees ("deg") |
| Bus | String | 1 (REQUIRED) |  |
| Node | Integer | 1 (REQUIRED) | Allowed values 1, 2 and 3 |
