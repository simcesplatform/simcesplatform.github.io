# Controller

## Description

This component is used to regulate the power setpoint of the resources based on information from economic dispatch.

## Subscribe
| Topic | Payload of interest |
| --- | --- |
| ResourceForecastState.Dispatch | Real Power and Reactive Power |

## Publish
| Topic | Payload of interest|
| --- | --- |
| ControlState.PowerSetpoint | Real Power and Reactive Power |

## Input Files
This component does not have any input files.

## Workflow
1. Simulation is started
2. Controller t receives SimState "running" 
3. Controller responds by sending Status "ready" message.
4. Controller  receives Epoch message for the new epoch
5. Controller  receives ResourceForecastState.Dispatch message from Economic Dispatch
6. Controller generates and publishes specific ControlState.PowerSetpoint message for the resources connected to it for the  current epoch
7. Controller sends Status "ready" message
8. Repeat steps for 6-7
9. If the controller receives SimState "stopped" closes by itself
