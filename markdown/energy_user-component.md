# User Component

## Names and locations

| Item | Value |
| - | - |
| Link to source code | <https://github.com/EVCommunities/Components/tree/main/user_component> |
| Management type | Platform managed |
| Docker image name | ghcr.io/evcommunities/user-component |
| Location of manifest file | <https://github.com/EVCommunities/Components/blob/main/component_manifest_user.yml> |


## Description

A component used to simulate a user that requests charging for an electric vehicle from stations. Holds information of a user, user’s car. Requests for charging of the car. Provides information on the state of charge.

## Messaging

### Subscribe

| Topic | Payload |
| --- | --- |
| PowerOutputTopic | Actual power provided from station. |


### Publish

| Topic | Payload |
| --- | --- |
| Init.User.CarMetadata | Car info of the user.  |
| User.UserState | User’s request details for charging.  |
| User.CarState  | State of charge of the car battery.   |


### Warnings

This component has no documentation about the publishing of warnings in result messages.


## Startup parameters

This component uses the block "UserComponent" in startup parameters.


## Input parameters

| Property | Datatype | Example |
| --- | --- | --- |
| user_id | Number | 1 |
| user_name | String | testUser1 |
| car_model | String | testModel1 |
| car_battery_capacity | Float | 120.0 (KWh) |
| car_max_power | Float | 22.0 (KW) |
| station_id | String | 1 |
| state_of_charge | Float | 20.0 (%) |
| target_state_of_charge | Float | 80.0 (%) |
| arrival_time | ISO 8601 datetime | 2023-01-11T12:00:00.000Z |
| target_time | ISO 8601 datetime | 2023-01-11T14:00:00.000Z |



## Input files

This component does not take any input file.


## Initialization workflow

This component does not have any initialization workflow.


## Epoch workflow

In each epoch, the component does the following:

1. User receives an Epoch message for a new epoch. The epoch number for the first epoch is 1. 
2. User publishes User.CarMetaData message if it is the epoch 1, the Intelligence Control component is the recipient of the message. 
3. User publishes User.UserState message which contains epoch no, user id, target SoC, target time. 
4. User listens to Station.PowerOutput message from the Station component which contains the actual power provided by the station. 
5. At the end of the epoch, User publishes User.CarState message which contains the epoch number, station id, user id, state of charge. 
6. User sends a Status message with value "ready". 



## Implementation details

### Language and platform

| Property | Value |
| --- | --- |
| Programming language | Python |
| Platform | Python 3.7.6 |
| Operating system | Docker Debian 10 (python:3.7.9) |


### External packages

The following packages are needed.

| Package | Version | Why needed | URL |
| --- | --- | --- | --- |
| Simulation Tools |  | Component implementation based on AbstractSimulationComponent. | <https://github.com/simcesplatform/simulation-tools> |
