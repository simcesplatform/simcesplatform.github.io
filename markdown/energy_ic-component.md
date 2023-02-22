# User Component

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

| Property | Datatype | Example |
| --- | --- | --- |
| total_max_power | Float | 30.0 |



## Input files

This component does not take any input file.


## Initialization workflow

This component does not have any initialization workflow.


## Epoch workflow

In each epoch, the component does the following:

1. IC receives an Epoch message for a new epoch. The epoch number for the first epoch is 1. 
2. IC listens to User.CarMetaData message if it is the epoch 1, stores all the user information when the message is received.
3. IC listens to StationStateTopic message, stores all the station information when the message is received.
4. IC listens to User.UserState message which contains epoch no, user id, target SoC, target time, stores the information to later determine the power requirement for the stations. 
5. After receiving all the messages (User.CarMetaData, StationStateTopic, User.UserState) from the users and stations from the epoch, IC publishes PowerRequirementTopic message which containers the power requirement for each station.
6. IC listens to User.CarState message from the User components which contains the updated state of charge of the car after receiving power from the station. 
7. IC sends a Status message with value "ready". 



## Implementation details

### Language and platform

| Property | Value |
| --- | --- |
| Programming language | Python |
| Platform | Python 3.7.6 |
| Operating system | Docker Debian 10 (python:3.7.6) |


### External packages

The following packages are needed.

| Package | Version | Why needed | URL |
| --- | --- | --- | --- |
| Simulation Tools |  | Component implementation based on AbstractSimulationComponent. | <https://github.com/simcesplatform/simulation-tools> |
| Domain tools |  | Uses the shared CSV file reading code. | <https://github.com/simcesplatform/domain-tools> |
| Domain messages |  | Uses the ResourceStateMessage class. | <https://github.com/simcesplatform/domain-messages> |
