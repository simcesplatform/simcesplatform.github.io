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

| Property | Datatype | Unit | Example |
| --- | --- | --- | --- |
| UserId | Integer | | 1 |
| UserName | String | | testUser1 |
| CarModel | String | | testModel1 |
| CarBatteryCapacity | Float | kWh | 120.0 |
| CarMaxPower | Float (> 0) | kW | 22.0 |
| StationId | String | | 1 |
| StateOfCharge | Float (0-100) | % | 20.0 |
| TargetStateOfCharge | Float (0-100) | % | 80.0 |
| ArrivalTime | ISO 8601 datetime | | 2023-01-11T12:00:00.000Z |
| TargetTime | ISO 8601 datetime | | 2023-01-11T14:00:00.000Z |



## Input files

This component does not take any input file.


## Initialization workflow

This component does not have any initialization workflow.


## Epoch workflow

In each epoch, the component does the following:

1. User receives an `Epoch` message for a new epoch. The epoch number for the first epoch is 1.
2. User publishes `User.CarMetaData` message if it is the epoch 1, the Intelligence Control component is the recipient of the message.
3. User publishes `User.UserState` message which contains epoch no, user id, target SoC, target time.
4. User listens for `Station.PowerOutput` message from the Station component which contains the actual power provided by the station.
5. After receiving `Station.PowerOutput` message, user publishes `User.CarState` message which contains the epoch number, station id, user id, and the state of charge at the end of the epoch.
6. User sends a `Status` message with value "ready".



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
