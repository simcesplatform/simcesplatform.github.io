# Controller

## Description

This component is used to regulate the power setpoint of the resources based on information from economic dispatch.

## Subscribe
| Exact Topic Name | Link to topic page(s) | Payload of interest|
| --- | --- | --- |
|ResourceForecastState.Dispatch| [energy_msg-resourceforecaststate-dispatch](energy_msg-resourceforecaststate-dispatch)| Real Power and Reactive Power |

## Publish
| Exact Topic Name | Link to topic page(s) | Payload of interest|
| --- | --- | --- |
|ControlState.PowerSetpoint| [energy_msg-ControlState.PowerSetpoint](energy_msg-ControlState.PowerSetpoint)| Real Power and Reactive Power |

## Input Files
This component does not have any input files.

## Workflow

ED component functionality is message triggered. The optimisation relies on input data and therefore we have a barrier for execution; all input data needs to be received before executing/solving the ED problem:

1. Simulation is started
2. Controller t receives SimState "running" 
3. Controller responds by sending Status "ready" message.
4. Controller  receives Epoch message for the new epoch
5. Controller  receives ResourceForecastState.Dispatch message from Economic Dispatch
6. Controller generates and publishes specific ControlState.PowerSetpoint message for the resources connected to it for the  current epoch
7. Controller sends Status "ready" message
8. Repeat steps for 6-7
9. If the controller receives SimState "stopped" closes by itself
