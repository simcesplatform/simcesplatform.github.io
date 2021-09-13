# DMSNetworkStatus.Current

## JSON structure
```nohighlight
{

  // *** Messages MUST be encoded in UTF-8! ***

  // --- Fields of AbstractMessage begin ---
  "Type" : "set-message-type-here",
  "Timestamp" : "2020-06-03T04:04:21.045Z",
  "SimulationId" : "2020-06-03T04:01:52.345Z",
  "SourceProcessId" : "set-process-id-here",
  "MessageId" : "set-message-id-here",
  // --- Fields of AbstractMessage end ---

  // --- Fields of AbstractResult begin ---
  "EpochNumber" : 14,
  "IterationStatus" : "final", // leave out when not iterating
  "LastUpdatedInEpoch" : 14, // OPTIONAL field
  "TriggeringMessageIds" : // TriggeringMessageIds are irrelevant in Epoch messages?
  [
    "epoch-14",
    "someprocess-14",
    "anotherprocess-14"
  ],
  "Warnings" : // If warnings precede an error, it can be beneficial to provide them as well in the Error message
  [
    "warning.convergence"
  ],
  // --- Fields of AbstractResult end ---

   "CurrentStatus":[
     {
       "DeviceId":"transformer",
       "Phase":1,
       "StatusSendingEnd":"acceptable",
       "StatusReceivingEnd":"acceptable",
       "ViolationSendingEnd":0,
       "ViolationReceivingEnd":0
     },
    {
     "DeviceId":"transformer",
     "Phase":2,
     "StatusSendingEnd":"unacceptable",
     "StatusReceivingEnd":"acceptable",
     "ViolationSendingEnd":10,
     "ViolationReceivingEnd":0
    }
   ]
}
```

## Fields and multiplicity

| Field | Type | Multiplicity | Notes |
| --- | --- | --- | --- |
| All fields from [AbstractResult](core_msg-abstractresult.md) | | | Fields from "abstract base class" |
| CurrentStatus | Current table block | 1 (REQUIRED) | |

### Current table block

| Field | Type | Multiplicity | Notes |
| --- | --- | --- | --- |
| DeviceId | String | 1 (REQUIRED) | DeviceIds are the same as defined in Init.NIS.NetworkComponentInfo |
| Phase | Integer | 1 (REQUIRED) | Phase values can be: 1, 2, 3 |
| StatusSendingEnd | String | 1 (REQUIRED) | State values can be:  acceptable,  close-to-limits,  unacceptable|
| StatusReceivingEnd | String | 1 (REQUIRED) | State values can be:  acceptable,  close-to-limits,  unacceptable|
| ViolationSendingEnd | Float | 1 (REQUIRED) | The value can be :  0 for acceptable and close-to-limits statues,  positive for over-loading|
| ViolationReceivingEnd | Float | 1 (REQUIRED) | The value can be :  0 for acceptable and close-to-limits statues,  positive for over-loading|



