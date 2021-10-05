# **Storage resource**

## Description

A component for simulating a resource which stores energy. The storage is given a initial state such as energy currently stored, maximum storage capacity and maximum power input and output. The storage is then controlled in each epoch by asking for desired power input or output. This control information can be read from a CSV file or received in each epoch from ControlState messages. 

The storage reports its state in each epoch with a ResourceState message containing the power input or output and the state of charge as a percentage. 

If the storage cannot fulfil the desired power for example there is not enough energy stored, the actual power the storage is capable of is reported and the message contains a warning. 

## Messaging

Subscribe

This component does not receive any result messages if a CSV file is used to control it.

If a CSV file is not used this component receives the following result messages:

| Topic | Payload |
| --- | --- |
| ControlState\.(process_id) | Power output or input required from the storage for the current epoch |


Publish

| Topic | Payload |
| --- | --- |
| ResourceState\.Storage\.\(process_id\) | Real power and state of charge for the epoch |

Warnings

This component may publish warnings in result messages as explained in the following table.

| Warning cause | When used | Related topic |
| --- | --- | --- |
| warning\.input\.range | Storage is unable to operate with the requested real power. The storage does not have enough energy, it cannot store enough energy or the power is more than the storage is rated for. | ResourceState\.Storage\.\(process_id\) |

## Startup parameters

This component uses the block "StorageResource" in startup parameters.

## Input files

The following table gives a list of the input files.

| Startup parameter for file | Description |
| --- | --- |
| ResourceStateCsvFile | When using a CSV file as control state source the file should contain the following columns: RealPower, ReactivePower and Bus. A optional Node column can be used. The file may include other columns which will be ignored by the component. ReactivePower is currently not used so it can be for example always zero. Each row containing values will then represent data for one epoch. There should be at least as many data rows as there will be epochs. Decimal separator is "." and column separator is by default "," which can be changed with the ResourceStateDelimiter startup parameter. |

## Initialization workflow

This component does not have any initialization workflow.

## Epoch workflow

In each epoch, the component does the following:

1. Get control state information from either:

    a. If a CSV file is not used receive a ContRolState message from topic ControlState\.process_id
	
	b. If a CSV file is used read next line from it

2. Calculate new state for the storage based on real power from control information and the duration of current epoch.
3. Create a ResourceState message from the new state of the storage notably real power and state of charge percentage.
4. If real power of storage state differs from control information add a warning\.input\.range warning to the message. This indicates that the storage could not operate according to the control power for example it was asked for too much energy.
5. Publish the ResourceState message to topic ResourceState\.Storage\.\(process_id\).
6. Publish status ready message.

## Implementation details

### Language and platform

|  |  |
| --- | --- |
| Programming language | Python |
| Platform | Python 3.7.6 |
| Operating system | Docker Debian 10 (python:3.7.6) |