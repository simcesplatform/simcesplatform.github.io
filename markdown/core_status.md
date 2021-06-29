# Status

## JSON structure

### Ready message

```nohighlight
{
  (Fields of AbstractMessage and AbstractResult must appear too!)
  
  "Value" : "ready"
}
```


### Error message

```nohighlight
{
  (Fields of AbstractMessage and AbstractResult must appear too!)
  
  "Value" : "error",
  "Description" : "Cannot calculate: expected pea soup because it is Thursday"
}
```


## Fields and multiplicity

| Field | Type | Multiplicity | Explanation |
|-|-|-|-|
| (All fields from [AbstractResult](core_abstractresult.md) and [AbstractMessage](core_abstractmessage.md)) | | | Fields from the "abstract base class" |
| Value | String | 1 (REQUIRED) | The status value being reported. This MUST be one of the following: "ready" or "error". |
| Description | String | 0..1 (OPTIONAL) | Status description; see (1) |

- (1)
    - If Value is "ready", this field SHOULD be omitted.
    - If Value is "error", this field SHOULD contain a description what went wrong.
