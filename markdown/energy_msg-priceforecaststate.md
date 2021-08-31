# PriceForecastState

## JSON structure

```nohighlight
{
  (Fields of AbstractMessage and AbstractResult must appear too!)

  "MarketId": "assign-market-here",
  "ResourceId": "assign-resource-here",
  "Prices":
  {
    "TimeIndex" :
    [
      "2020-02-17T10:00:00Z",
      "2020-02-17T11:00:00Z",
      "2020-02-17T12:00:00Z"
    ],
    "Series" :
    {
      "Price" :
      {
        "UnitOfMeasure" : "{EUR}/(kW.h)",
        "Values" :
        [
          0.041,
          0.042,
          0.043
        ]
      }
    }
  }
}
```


## Fields and multiplicity

| Field | Type | Multiplicity | Explanation |
|-|-|-|-|
| (All fields from [AbstractResult](core_msg-abstractresult.md) and [AbstractMessage](core_msg-abstractmessage.md)) | | | Fields from the "abstract base class" |
| MarketId | String | 1 (REQUIRED) | Market identifier |
| ResourceId | String | 0..1 (OPTIONAL) | The resource related to the price. If there is no resource specified, the message MUST contain market prices. |
| PricingType | ? | 0..1 (OPTIONAL) | ? |
| Prices | Time series block | 1 (REQUIRED) | See (1) below |

(1) Series included in the block:

- REQUIRED: "Price"
    - Unit of measure: "{EUR}/(kW.h)" (or "{EUR}/(MW.h)"?)
    - Currencies are not supported in UCUM, therefore "EUR" is an annotation in curly brackets
- OPTIONAL: variance or similar (TBD!)
