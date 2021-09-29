# Static time series resource

## Names and locations

| Item | Value |
| - | - |
| Link to source code | TODO |
| Management type | Platform managed |
| Docker image name | TODO |
| Location of manifest file | TODO |


## Description

A component used to simulate simple loads and generators whose published states are determined by a file containing a simple time series of attribute values for each epoch.

 

## Messaging

Please note that all topics are specified on the page [Topics (energy)](energy_topics.md). Refer to this page for exact topic patterns and the related message structures.


### Subscribe

This component does not receive any result messages.


### Publish

| Topic | Payload |
| --- | --- |
| [ResourceState.(ResourceCategory).(ResourceId)](energy_topic-resourcestate.md) | State of the resource including real power, reactive power, bus and node for the current epoch. |


### Warnings

This component has no documentation about the publishing of warnings in result messages.


## Startup parameters

This component uses the block "StaticTimeSeriesResource" in startup parameters.


## Input files

The following table gives a list of the input files.


| Startup parameter for file | Description |
| --- | --- |
| ResourceStateFile | The CSV file should contain columns named after the ResourceState message attributes: RealPower, ReactivePower, CustomerId and Node. The Node column is optional. Each row containing values will then represent data for one epoch. There should be at least as many data rows as there will be epochs. The file may contain other columns which the component ignores. The column separator is by default comma "," and it can be changed with the ResourceFileDelimiter startup parameter. |


## Initialization workflow

This component does not have any initialization workflow.


## Epoch workflow

In each epoch, the component does the following:

1. Read next line from resource state CSV file.
2. Create a ResourceState message from the CSV line and publish it.
3. Publish ready message


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
| Domain tools |  | Uses the shared CSV file reading code. | TODO |
| Domain messages |  | Uses the ResourceStateMessage class. | TODO |
