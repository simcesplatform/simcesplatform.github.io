# ResourceForecastState.Power

## JSON structure

```nohighlight
{
  (Fields of AbstractMessage and AbstractResult must appear too!)

  "ResourceId" : "load1",
  "Forecast" :
  {
    "TimeIndex" :
    [
      "2020-06-25T00:00:00Z",
      "2020-06-25T01:00:00Z",
      "2020-06-25T02:00:00Z",
      "2020-06-25T03:00:00Z"
    ],
    "Series" :
    {
      "RealPower" :
      {
        "UnitOfMeasure" : "kW",
        "Values" :
        [
          -0.2,
          -0.27,
          -0.15,
          -0.21
        ]
      }
    }
  }
}
```

Example file: [ResourceStateForecast-example.json](json/ResourceStateForecast-example.json)

## Fields and multiplicity

| Field | Type | Multiplicity | Explanation |
| --- | --- | --- | --- |
| All fields from [AbstractResult](core_msg-abstractresult.md) | | | Fields from "abstract base class" |
| ResourceId | String | 1 (REQUIRED) | Reference to resource "name" |
| Forecast | Time series block | See (1) |  |

(1):

- "RealPower": 1 (REQUIRED); unit of measure "kW"
- "RealPowerVariance" OR "RealPowerStandardDeviation": 0..1 (OPTIONAL)
- "ReactivePower": 0..1 (OPTIONAL); unit of measure "kV.A{r}"
- "ReactivePowerVariance" OR "ReactivePowerStandardDeviation": 0..1 (OPTIONAL) if "ReactivePower" exists)
