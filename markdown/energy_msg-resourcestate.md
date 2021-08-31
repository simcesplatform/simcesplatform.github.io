# ResourceState

## JSON structure

```nohighlight
{
  (Fields of AbstractMessage and AbstractResult must appear too!)

  "Bus" : "loadbus1",
  "RealPower" : { "Value": 100.0, "UnitOfMeasure": "kW" },
  "ReactivePower" : { "Value": 0.0, "UnitOfMeasure": "kV.A{r}" },
  "Node" : 2, // Optional
  "StateOfCharge" : { "Value": 68.1, "UnitOfMeasure": "%" } // Optional
}
```


## Fields and multiplicity

| Field | Type | Multiplicity | Explanation |
|-|-|-|-|
| (All fields from [AbstractResult](core_msg-abstractresult.md) and [AbstractMessage](core_msg-abstractmessage.md)) | | | Fields from the "abstract base class" |
| Bus | String | 1 (REQUIRED) | Name of bus to which the resource is connected. |
| RealPower | Quantity block | 1 (REQUIRED) | 'Towards the grid' is positive. Always in "kW". |
| ReactivePower | Quantity block | 1 (REQUIRED) | Reactive power. Always in "kV.A{r}" |
| Node | Integer | 0..1 (OPTIONAL) | Node that 1-phase resource is connected to. Possible values 1, 2 and 3. If this is not specified then it is assumed that the resource is 3-phase resource. |
| StateOfCharge | Quantity block | 0..1 (OPTIONAL) | Present amount of energy stored, % of rated kWh. Unit of measure: "%". |
