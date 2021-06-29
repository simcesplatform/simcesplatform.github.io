# Epoch message

This page specifies the "Epoch" message.


## JSON structure

```nohighlight
{
  (Fields of AbstractMessage and AbstractResult must appear too!)

  "StartTime" : "2020-06-03T13:00:00Z",
  "EndTime" : "2020-06-03T14:00:00Z"
}
```


## Fields and multiplicity

| Field | Type | Multiplicity | Explanation |
|-|-|-|-|
| (All fields from [AbstractResult](core_msg-abstractresult.md) and [AbstractMessage](core_msg-abstractmessage.md)) | | | Fields from the "abstract base class" |
| StartTime | ISO 8601 (Date and time, UTC zone) | 1 (REQUIRED) | Simulated start time of the epoch |
| EndTime | ISO 8601 (Date and time, UTC zone) | 1 (REQUIRED) | Simulated end time of the epoch |
