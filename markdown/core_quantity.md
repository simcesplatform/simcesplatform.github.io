# Quantity block

Quantity block is neither a topic nor message but a JSON block that can be included in any message. Use the quantity block whenever there is a measurement value or another quantity with an associated unit of measure.


## JSON structure

```nohighlight
{
  "Value": 12.3,
  "UnitOfMeasure": "kW"
}
```


## Fields and multiplicity

| Field | Type | Multiplicity | Explanation |
|-|-|-|-|
| UnitOfMeasure | String | 1 (REQUIRED) | Unit of measure. This SHOULD follow the [UCUM specification](core_ucum.md). |
| Value | Float | 1 (REQUIRED) | Value |
