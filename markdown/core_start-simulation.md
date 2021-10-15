# Starting a general simulation

To start a new simulation run:

- Specify the simulation by creating a configuration file. ([Specifying the simulation configuration file](core_start-simulation.md#specifying-the-simulation-configuration-file))
- Use the start script to start the simulation. ([Starting a new simulation run](core_start-simulation.md#starting-a-new-simulation-run))

## Specifying the simulation configuration file

The simulation configuration file specifies parameters for the core platform components, for example the epoch length, and defines all the components and their parameters that will participate in the simulation.

The configuration file uses [YAML](https://en.wikipedia.org/wiki/YAML) format and has two sections: `Simulation` for specifying the core platform parameters and `Components` for specifying the participating components and their parameters. The component section has a close relation to the [Start message](core_msg-start.md). YAML uses Python style indentation to indicate nesting and it is advisable to be consistent in the use of the indentation. All the examples given here use indentation with 4 white spaces.

Example configuration file with an explanation for the specified simulation is found at the section [Example simulation configuration file](core_start-simulation.md#example-simulation-configuration-file). Configuration file template and additional specification is found at the section [Simulation configuration file specification](core_start-simulation.md#simulation-configuration-file-specification).

## Example simulation configuration file

Example simulation configuration file for a simulation where a Grid component has been added to the EC scenario can be found at [simulation_configuration_grid.yml](https://github.com/simcesplatform/Platform-Manager/blob/master/simulation_configuration_grid.yml):

```yaml
Simulation:
    Name: "Energy community with Grid demo"
    Description: "This scenario includes a grid simulation with OpenDSS."
    InitialStartTime: "2020-06-25T00:00:00.000+03:00"
    EpochLength: 3600
    MaxEpochCount: 24

    # Optional settings for the Simulation Manager
    ManagerName: "Manager"
    EpochTimerInterval: 20
    MaxEpochResendCount: 2

    # Optional settings for the Log Writer
    MessageBufferMaxDocumentCount: 10
    MessageBufferMaxInterval: 5.0

Components:  # these are the names of the component implementations (defined in the supported components JSON file)
    # duplication_count is reserved keyword and cannot be used as a parameter for a component instance

    Grid:                            # The externally managed Grid component type
        Grid:                        # The name of the Grid instance in the simulation run
            ModelName: "EC_Network"  # The grid model name that will be sent as a part of the Start message
            Symmetrical: false

    Dummy:                        # The platform-managed Dummy component to slow down the simulation
        dummy:                    # The base name for the Dummy components
            duplication_count: 2  # Create 2 components, named dummy_1 and dummy_2, with otherwise identical parameters
            MinSleepTime: 1
            MaxSleepTime: 5

    StaticTimeSeriesResource:    # The platform-managed Static Time Series Resource component
        load1:                   # The name of this StaticTimeSeriesResource instance in the simulation run
            ResourceType: "Load"                       # corresponds to RESOURCE_TYPE environment variable
            ResourceStateFile: "/resources/Load1.csv"  # corresponds to RESOURCE_STATE_FILE environment variable
        load2:
            ResourceType: "Load"
            ResourceStateFile: "/resources/Load2.csv"
        load3:
            ResourceType: "Load"
            ResourceStateFile: "/resources/Load3.csv"
        load4:
            ResourceType: "Load"
            ResourceStateFile: "/resources/Load4.csv"
        ev:
            ResourceType: "Load"
            ResourceStateFile: "/resources/EV.csv"
        pv_small:
            ResourceType: "Generator"
            ResourceStateFile: "/resources/PV_small.csv"
        pv_large:
            ResourceType: "Generator"
            ResourceStateFile: "/resources/PV_large.csv"
```

Note that, while string values can be in most cases be given without double quotes in YAML files, they are consistently used in the above example to avoid any possible edge cases where the strings without quotes might be interpreted as something other than a single string value.

A breakdown of parameters set in the example configuration using the knowledge of the default values from [Start message](core_msg-start.md) and component manifest files for the participating components is given below. A more detailed explanation on what each parameter means for the used components can be found on the [Start message](core_msg-start.md) page.

- The required overall parameters
    - The simulation name is set to "`Energy community with Grid demo`"
    - The description for the simulation is set to "`This scenario includes a grid simulation with OpenDSS.`"
    - The start time for the first epoch in set to "`2020-06-25T00:00:00.000+03:00`" or to "`2020-06-24T21:00:00.000Z`" in UTC time
    - The duration of each epoch is set to `3600` seconds, i.e. 1 hour
    - Maximum number of epochs is set to `24`, i.e. the simulation will last 24 hours or 1 day
- The optional overall parameters
    - The Simulation Manager name in the simulation is set to "`Manager`", the default name would be "`SimulationManager`"
    - The time duration until Simulation Manager tries to resend the epoch message is set to `20` seconds, the default duration would be `120` seconds
    - The maximum number of epoch resends the Simulation Manager is allowed to try before ending the simulation is set to `2` epoch resends, the default would be `5` epoch resends
    - The maximum number of messages the buffer in Log Writer can have is set to `10` messages, the default would be `20` messages
    - The maximum time interval until the message buffer in Log Writer is cleared is set to `5` seconds, the default would be `10` seconds
- The simulation will have one Grid component participating that is externally managed
    - The name of the Grid component is set to "`Grid`"
    - The `ModelName` parameter for the Grid is set to "`EC_Network`"
    - The `Symmetrical` parameter for the Grid is set to the boolean value `false`
    - The optional parameter `MaxControlCount` for the Grid will be set to the default value `15`
    - The optional parameter `ModelVoltageBand` for the Grid will be set to the default value `0.15`
- The simulation will have two Dummy components that are platform managed
    - Using the `duplicate_count` attribute there will be `2` Dummy components with identical parameters created. The component names will be in the format `<basename>_<number>`, i.e. "`dummy_1`" and "`dummy_2`" since the base name is set to "`dummy`"
    - The `MinSleepTime` parameter for the Dummy components is set to `1`
    - The `MaxSleepTime` parameter for the Dummy components is set to `5`
    - All the other parameters for the Dummy components, i.e. `WarningChance`, `SendMissChance`, `ReceiveMissChance` and `ErrorChance`, are set to the their default value of `0.0`
- The simulation will have 7 Static Time Series Resource components that are platform managed
    - The resource components representing loads are named "`load1`" "`load2`" "`load3`" "`load4`" and "`ev`"
        - The `ResourceType` parameter is set to "`Load`" in all load components
        - The `ResourceStateFile` parameter is set to the filename where the resource component will find it after it has been deployed, for example the filename for the "`load1`" component is set to "`/resources/Load1.csv`"
            - Note, that the filename given here refer to the filenames inside the Docker containers and thus they all include "`/resources`" at the beginning, as will the case when they are made available according to the section [Making input files available for the platform](core_run-preparations.md#making-input-files-available-for-the-platform).
    - The resource components representing generators are named "`pv_small`" and "`pv_large`"
        - The `ResourceType` parameter is set to "`Generator`" in both generator components
        - The `ResourceStateFile` parameter is set to the filename where the resource component will find it after it has been deployed, for example the filename for the "`pv_small`" component is set to "`/resources/PV_small.csv`"

## Simulation configuration file specification

The YAML configuration for a simulation run uses the following format:

```yaml
Simulation:
    # Parameters for the core components

Components:
    # Definitions for all the participating components and their parameters
```

File [simulation_configuration_template.yml](https://github.com/simcesplatform/Platform-Manager/blob/master/simulation_configuration_template.yml) contains a template that can be used as a base for defining new simulations. The template file contains comments that explain the structure of the file.

The definitions for the participating components have a correspondence to the process parameters blocks in the Start message An example Start message that corresponds to the template configuration is given in [instructions/start_message_template.json](https://github.com/simcesplatform/Platform-Manager/blob/master/instructions/start_message_template.json).

## Starting a new simulation run

To start a new simulation run, use Bash compatible terminal to navigate to the installation folder and use the command

```bash
source start_simulation.sh <configuration_file>
```

where `<configuration_file>` is the filename containing the YAML configuration for the simulation run.
