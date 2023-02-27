# Station Component

## Names and locations

| Item | Value |
| - | - |
| Link to source code | <https://github.com/EVCommunities/Components/tree/main/station_component> |
| Management type | Platform managed |
| Docker image name | ghcr.io/evcommunities/station-component |
| Location of manifest file | <https://github.com/EVCommunities/Components/blob/main/component_manifest_station.yml> |


## Description

A component used to simulate a station that receives requests from user components for charging for electric vehicles. Holds information of a station. Provides information of actual power provided by stations.



## Messaging


### Subscribe

| Topic | Payload |
| --- | --- |
| PowerRequirementTopic | Charging requirements from Intelligence Controller. |


### Publish

| Topic | Payload |
| --- | --- |
| StationStateTopic | Station info of the station.  |
| PowerOutputTopic | Actual power provided from station.  |


### Warnings

This component has no documentation about the publishing of warnings in result messages.


## Startup parameters

This component uses the block "StationComponent" in startup parameters.

## Input parameters

| Property | Datatype | Unit | Example |
| --- | --- | --- | --- |
| StationId | String | | 1 |
| MaxPower | Float (> 0) | kW | 22.0 |


## Input files

This component does not take any input file.


## Initialization workflow

This component does not have any initialization workflow.


## Epoch workflow

In each epoch, the component does the following:

1. Station receives an `Epoch` message for a new epoch. The epoch number for the first epoch is 1.
2. Station publishes `Station.StationState` message which contains epoch number, station id, and max power.
3. Station listens to `IntelligenceControl.PowerRequirement` message from the IntelligenceControl component which contains the charging requirements (power) that needs to be provided by the station.
4. Station publishes `Station.PowerOutput` message which contains the actual power provided by the station.
5. Station sends a `Status` message with value "ready".



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
