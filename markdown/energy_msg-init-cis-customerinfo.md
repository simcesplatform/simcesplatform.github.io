# Init.CIS.CustomerInfo

## Description

This message contains the list of electricity distribution components to be delivered in simulation initialization stage.

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
  "ResourceId" : [
    "load1",
    "load2",
    "load3"
  ],
  "CustomerId" : [
    "GridA-1",
    "GridA-1",
    "GridA-1"
  ]
  "BusName":[
    "2",
    "1",
    ""
  ]
}
```

## Fields and multiplicity

| Field | Type | Multiplicity | Notes |
| --- | --- | --- | --- |
| All fields from [AbstractResult](core_msg-abstractresult.md) | | | Fields from "abstract base class" |
| ResourceId | Array of strings | N by 1 (REQUIRED) | Refers to ProcessId of Resources EXC processes |
| CustomerId | Array of strings | N by 1 (REQUIRED) | Grid allocated customer identifier |
| BusName | Array of strings | N by 1 (REQUIRED) | Connected system bus name, refers to bus name in [Init.NIS.NetworkBusInfo](energy_msg-init-nis-networkbusinfo.md) **NOTE**: This can be an empty string. It means that Grid (DSS) has given a resource a customer id but it doesn't consider it in loadflow calculation. This is to facilitate resources that might be considered as a part of Economic Dispatch but that are not located in modelled grid |




