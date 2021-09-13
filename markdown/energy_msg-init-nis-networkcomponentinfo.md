# Init.NIS.NetworkComponentInfo

##Description

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


"PowerBase" : {
  "Value" : 10,
  "UnitOfMeasure" : "kV.A"
},
 
"DeviceId" : [ 
  "t1",
  "l1",
  "l2",
  "l3"
],

"SendingEndBus" : [
  "sourcebus",
  "1",
  "2",
  "3",
  "4"
],

"ReceivingEndBus" : [
  "1",
  "2",
  "3",
  "4"
],

"Resistance" : {
  "UnitOfMeasure": "{pu}",
  "Values" : [
  0.172,
  0.056,
  0.284,
  0.054
  ]
},

"Reactance" : {
  "UnitOfMeasure": "{pu}",
  "Values" : [
  0.134,
  0.008,
  0.027,
  0.036
  ]
},

"ShuntAdmittance" : {
  "UnitOfMeasure": "{pu}",
  "Values" : [
  0,
  0,
  0,
  0
  ]
},

"ShuntConductance" : {
  "UnitOfMeasure": "{pu}",
  "Values" : [
  0.00356,
  0,
  0,
  0
  ]
},
"RatedCurrent" : {
  "UnitOfMeasure": "{pu}",
  "Values" : [
  1.739130435,
  1.22,
  0.42,
  1
  ]
}
}
```

## Fields and multiplicity

| Field | Type | Multiplicity | Notes |
| --- | --- | --- | --- |
| All fields from [AbstractResult](core_msg-abstractresult.md) | | | Fields from "abstract base class" |
| PowerBase | [Quantity block](core_block-quantity.md) | 1 (REQUIRED) | Power base value, always in "kV.A"|
| DeviceId | Array of strings | N by 1 (REQUIRED) | List of device identifiers. NetworkState.Current and NetworkState.Loss messages refer to these.|
| SendingEndBus | Array of strings | N by 1 (REQUIRED) | |
| ReceivingEndBus | Array of strings | N by 1 (REQUIRED) | |
| Resistance | [Quantity array block](core_block-quantity-array.md) | N by 1 ((REQUIRED), single element can be NULL) | Series resistance for the device, in per unit "{pu}" (sending end base) |
| Reactance | [Quantity array block](core_block-quantity-array.md) | N by 1 ((REQUIRED), single element can be NULL) | Series reactance for the device, in per unit "{pu}" (sending end base) |
| ShuntAdmittance | [Quantity array block](core_block-quantity-array.md) | N by 1 ((REQUIRED), single element can be NULL) | Shunt Admittance for the device, in per unit "{pu}" (sending end base) |
| ShuntConductance | [Quantity array block](core_block-quantity-array.md) | N by 1 ((REQUIRED), single element can be NULL) | Shunt Conductance for the device, in per unit "{pu}" (sending end base) |
| RatedCurrent | [Quantity array block](core_block-quantity-array.md) | N by 1 ((REQUIRED), single element can be NULL) | Rated current, in per unit "{pu}" (sending end base) |




