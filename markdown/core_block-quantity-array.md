# Quantity array block

Quantity array block is neither a topic nor message but a JSON block that can be included in any message. Use it whenever there is an array of measurement values or another quantities with an associated unit of measure.


## JSON structure

```nohighlight
{
  "UnitOfMeasure": "kW",
  "Values":
  [
    "0.1",
    "0.2",
    "0.3",
    "0.4"
  ]
}
```


## Fields and multiplicity

| Field | Type | Multiplicity | Explanation |
|-|-|-|-|
| UnitOfMeasure | String | 1 (REQUIRED) | Unit of measure. This SHOULD follow the [UCUM specification](core_ucum.md). |
| Value | Array of floats | 1 (REQUIRED) | The values. The array MUST exist, but the number of values is arbitrary. |
