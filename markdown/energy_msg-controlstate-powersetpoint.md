# ControlState.PowerSetpoint

## JSON structure

```nohighlight
{
  {
  (Fields of AbstractMessage and AbstractResult must appear too!)
  "RealPower" : { "Value": 100.0, "UnitOfMeasure": "kW" },
  "ReactivePower" : { "Value": 0.0, "UnitOfMeasure": "kV.A{r}" }
  }
```


## Fields and multiplicity

| Field | Type | Multiplicity | Explanation |
|-|-|-|-|
| PowerSetpoint | PowerSetpoint block (see below)| 1 | Power setpoint for next epoch |

## PowerSetpoint block

| Field | Type | Multiplicity | Explanation |
|-|-|-|-|
| RealPower | Quantity block| 1 | Real power setpoint for next epoch,Unit of measure: "kW"|
| ReactivePower | Quantity block| 1 | Reactive power setpoint for next epoch,Unit of measure: "kV.A{r}"|


