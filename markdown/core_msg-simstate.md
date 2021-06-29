# SimState message

This message notifies about starting and ending a simulation run.

## JSON structure

```nohighlight
{
  (Fields of AbstractMessage and AbstractResult must appear too!)
  
  "SimulationState" : "running",
  "Name" : "Name of the simulation",
  "Description" : "Longer description about the simulation"
}
```


## Fields and multiplicity

| Field | Type | Multiplicity | Explanation |
|-|-|-|-|
| (All fields from [AbstractMessage](core_msg-abstractmessage.md)) |  |  | Fields from the "abstract base class" |
| SimulationState | String | 1 (REQUIRED) | Simulation state, either "running" or "stopped" |
| Name | String | 0..1 (OPTIONAL) | A human-friendly name for the simulation |
| Description | String | 0..1 (OPTIONAL) | A longer description of the simulation run meant for humans |
