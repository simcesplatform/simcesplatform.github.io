# Time series block

Time series block is neither a topic nor message but a JSON block that can be included in any message.

## JSON structure

```nohighlight
{
  "TimeIndex" :
  [
    "2020-02-17T10:00:00Z",
    "2020-02-17T11:00:00Z",
    "2020-02-17T12:00:00Z"
  ],
  "Series" :
  {
    "MagnitudeX" :
    {
      "UnitOfMeasure" : "cm",
      "Values" :
      [
        1.4,
        1.7,
        1.6
      ]
    },
    "MagnitudeY" :
    {
      "UnitOfMeasure" : "Cel",
      "Values" :
      [
        -4.2,
        -3.7,
        -3.1
      ]
    }
  }
}
```


## Fields and multiplicity

| Field | Type | Multiplicity | Explanation |
|-|-|-|-|
| TimeIndex | Array of ISO 8601 date and time; UTC zone | 1 (REQUIRED) | The time of each value in the time series. These MUST be ordered from lowest to highest (i.e., ascending). |
| Series | Series block | 1..* (REQUIRED, can be many) | The actual time series |



### Series block

| Field | Type | Multiplicity | Explanation |
|-|-|-|-|
| UnitOfMeasure | String | 1 (REQUIRED) | Unit of measure. This SHOULD follow the [UCUM specification](core_ucum.md). |
| Values | Array of basic type (JSON string, number or boolean); complex numbers not supported out of the box | 1 (REQUIRED) | Value for each time. The array length MUST be equal to the length of TimeIndex array. |
