# Simulation-specific exchanges



| Item | Value | Comment |
|-|-|-|
| Type        | topic | Necessary for topic-based communication |
| Name        | procem.[unique-id-derived-from-simulation-id] | The exchange name is decided by PlatformManager at simulation startup. |
| Durable     | false | The exchange would disappear in a broker restart. The intention is that the simulation should not continue if the broker restarts. Instead, there should be another simulation run. |
| Auto delete | true | The exchange will be deleted when no longer in use (i.e., after the simulation run). This enables an automatic cleanup of unused resources. Please note that this value is independent of the "auto delete" flag of message queues! |
