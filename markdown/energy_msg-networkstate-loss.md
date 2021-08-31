# NetworkState.Loss

## JSON structure

```nohighlight
{
  (Fields of AbstractMessage and AbstractResult must appear too!)

  ...
  "DeviceId" : "xyz-3"
}
```


## Fields and multiplicity

| Field | Type | Multiplicity | Explanation |
|-|-|-|-|
| (All fields from [AbstractResult](core_msg-abstractresult.md) and [AbstractMessage](core_msg-abstractmessage.md)) | | | Fields from the "abstract base class" |
| RealPowerShunt | Quantity block | 1 (REQUIRED) | Voltage-dependent losses (e.g. transformer core effect losses) |
| RealPowerSeries | Quantity block | 1 (REQUIRED) | Current dependent losses (e.g. line losses) |
| ReactivePowerShunt	 | Quantity block | 0..1 (OPTIONAL) | 	Voltage-dependent losses (e.g. transformer magnetization current) |
| ReactivePowerSeries	 | Quantity block | 0..1 (OPTIONAL) | Current dependent losses (e.g. transformer stray inductance) |
| DeviceId | String | 1 (REQUIRED) | Must correspond to an entry in network model branches |
