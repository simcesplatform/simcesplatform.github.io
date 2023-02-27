# Intelligent Controller Component

## Names and locations

| Item | Value |
| - | - |
| Link to source code | <https://github.com/EVCommunities/Components/tree/main/ic_component> |
| Management type | Platform managed |
| Docker image name | ghcr.io/evcommunities/ic-component |
| Location of manifest file | <https://github.com/EVCommunities/Components/blob/main/component_manifest_ic.yml> |


## Description

A controller component that holds all the logic and algorithm for processing user charging requirements and calculating power outputs for stations.

## Messaging

### Subscribe

| Topic | Payload |
| --- | --- |
| Init.User.CarMetadata | Car info of the user.  |
| User.UserState | User’s request details for charging.  |
| User.CarState  | State of charge of the car battery.   |
| StationStateTopic | Station info of the station. |


### Publish

| Topic | Payload |
| --- | --- |
| PowerRequirementTopic |  Charging requirements from Intelligence Controller. |


### Warnings

This component has no documentation about the publishing of warnings in result messages.


## Startup parameters

This component uses the block "ICComponent" in startup parameters.


## Input parameters

| Property | Datatype | Unit | Example |
| --- | --- | --- | --- |
| TotalMaxPower | Float | kW | 30.0 |



## Input files

This component does not take any input file.


## Initialization workflow

This component does not have any initialization workflow.


## Epoch workflow

In each epoch, the component does the following:

1. IC receives an `Epoch` message for a new epoch. The epoch number for the first epoch is 1.
2. IC listens to `User.CarMetaData` messages if it is the epoch 1, stores all the user information when a message is received.
3. IC listens to `StationStateTopic` messages, stores all the station information when a message is received.
4. IC listens to `User.UserState` messages which contains epoch number, user id, target SoC, and target time. IC stores the information in order to later determine the power requirement for the stations.
5. After receiving all the messages (`User.CarMetaData`, `StationStateTopic`, `User.UserState`) from all the users and stations, IC makes a plan for distributing the available charging power and publishes `PowerRequirementTopic` messages which containers the power requirements for each station.
6. IC listens to `User.CarState` messages from the User components which contains the updated state of charges for the cars after receiving power from the station.
7. After having received all `User.CarState` messages, IC sends a `Status` message with value "ready".



## Implementation details

### Language

| Property | Value |
| --- | --- |
| Programming language | Python 3.7.9 |


### External packages

The following packages are needed.

| Package | Version | Why needed | URL |
| --- | --- | --- | --- |
| Simulation Tools |  | Component implementation based on AbstractSimulationComponent. | <https://github.com/simcesplatform/simulation-tools> |
