# Init.NIS.NetworkBusInfo

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

  "BusName" : [
    "sourcebus",
    "1",
    "2",
    "3"
  ],
  "BusType" : [
    "root",
    "dummy",
    "dummy",
    "usage-point"
  ],
  "BusVoltageBase" :
  {
    "UnitOfMeasure": "kV",
    "Values": [
      20,
      0.4,
      0.4,
      0.4
    ]
  }
}
```

## Fields and multiplicity

| Field | Type | Multiplicity | Notes |
| --- | --- | --- | --- |
| All fields from [AbstractResult](core_msg-abstractresult.md) | | | Fields from "abstract base class" |
| BusName | Array of strings | N by 1 (REQUIRED) | System bus name, referenced by NetworkState.Voltage |
| BusType | Array of strings (enumerated) | N by 1 (REQUIRED) | Allowed values for elements: **"dummy"** : This bus has no resources connected to it, it only has a topology purpose, **"root"** : This is the feed-in bus of the network, it is assumed to be connected to a constant voltage source. **"usage-point"** : This bus has resource(s) connected to it, power will be injected to or draw fron grid at this bus|
| BusVoltageBase | [Quantity array block](core_block-quantity-array.md)  | N by 1 (REQUIRED) | Unit of measure is always "kV" |




