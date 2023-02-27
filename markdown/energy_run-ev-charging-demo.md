# Running the EV Charging demo scenario simulation

After successfully [running the Energy Community demo scenario simulation](energy_run-ec-demo.md), an additional externally developed test scenario can be tried. The scenario description can be found at [Electric vehicle charging demo](energy_scenario-ev-charging-demo.md).

## Installing the core components

Instructions for installing core components: [Instructions for installing core components](core_install.md)

## Installing the required domain components

To be able to use the [User](energy_user-component.md), [Station](energy_station-component.md), [Intelligent Controller](energy_ic-component.md) components in a simulation run, they must first be installed. The general instruction for installing components can be found at [Running a simulation](core_run.md#starting-a-simulation). The following steps are made specifically for the EV charging demo scenario.

1. Ensure that the file `docker_images_domain.txt` contains the Docker image names for the following components:
    - UserComponent: `ghcr.io/evcommunities/user-component:latest`
    - StationComponent: `ghcr.io/evcommunities/station-component:latest`
    - ICComponent: `ghcr.io/evcommunities/ic-component:latest`

    Example `docker_images_domain.txt` file after modifications:

        :::txt
        ghcr.io/simcesplatform/static-time-series-resource:latest
        ghcr.io/evcommunities/user-component:latest
        ghcr.io/evcommunities/station-component:latest
        ghcr.io/evcommunities/ic-component:latest

2. Ensure that the file `components/github_server_domain.yml` contains in the `Repositories` list the code repository for User, Station and IC: `EVCommunities/Components` as well as the information about the manifest files for each component.

    Example `components/github_server_domain.yml` file after modifications:

        :::yaml
        Repositories:
            - simcesplatform/static-time-series-resource
            - EVCommunities/Components:
                 File: component_manifest_user.yml
                 Branch: main
            - EVCommunities/Components:
                 File: component_manifest_station.yml
                 Branch: main
            - EVCommunities/Components:
                 File: component_manifest_ic.yml
                 Branch: main
        Type: GitHub

3. Using Bash compatible terminal (Git Bash in Windows) navigate to the `platform` folder (the folder where the platform is installed).

4. Install components along with the other domain components by using the command:

        :::bash
        source platform_domain_setup.sh

    This script will fetch the Docker images and component manifest files for the domain components.

5. Copy the ready-made simulation configuration file from [simulation_configuration_ev_charging.yml](https://github.com/EVCommunities/Components/blob/main/simulation_configuration_ev_charging.yml) to the `platform` folder.
    - Either copy the file manually or use the following command line instruction:

            :::bash
            wget https://raw.githubusercontent.com/EVCommunities/Components/main/simulation_configuration_ev_charging.yml

6. (Optional) Look through the simulation configuration file and see how it corresponds to the scenario description at [EV charging demo scenario](energy_scenario-ev-charging-demo.md).

## Starting the simulation run

To start the EV charging demo scenario simulation, use Bash compatible terminal to navigate to the `platform` folder and use the command

    :::bash
    source start_simulation.sh simulation_configuration_ev_charging.yml

To follow the simulation run while it is running see the page: [Following a running simulation](core_follow-run.md)

## Checking the results from the simulation run

After the simulation run has been completed you should be able to use the [Log Reader](core_log-api.md) to look through the messages used in the simulation. A couple of examples of using the Log Reader API are given below. In all examples, `<simulation_id>` should be replaced by the simulation id given by the Platform Manager.

- Get a list of messages sent from the Intelligent controller component (ic1) at epoch 1.

    - Request:

        [`http://localhost:8080/simulations/<simulation_id>/messages?process=ic1&epoch=1`](http://localhost:8080/simulations/<simulation_id>/messages?process=ic1&epoch=1)

    - Response:

            :::json
            [
                {
                    "AffectedUsers": [
                        "uc2",
                        "uc1",
                        "uc4"
                    ],
                    "AvailableEnergy": 86.05851979345955,
                    "EpochNumber": 1,
                    "MessageId": "ic1-2",
                    "SimulationId": "2023-02-23T08:31:20.924Z",
                    "SourceProcessId": "ic1",
                    "Timestamp": "2023-02-23T08:31:37.604000Z",
                    "Topic": "Requirements.Warning",
                    "TriggeringMessageIds": [
                        "SimulationManager-2"
                    ],
                    "Type": "RequirementsWarning"
                },
                {
                    "EpochNumber": 1,
                    "MessageId": "ic1-3",
                    "Power": 20,
                    "SimulationId": "2023-02-23T08:31:20.924Z",
                    "SourceProcessId": "ic1",
                    "StationId": "1",
                    "Timestamp": "2023-02-23T08:31:37.607000Z",
                    "Topic": "PowerRequirementTopic",
                    "TriggeringMessageIds": [
                        "SimulationManager-2"
                    ],
                    "Type": "PowerRequirement",
                    "UserId": 1
                },
                {
                    "EpochNumber": 1,
                    "MessageId": "ic1-4",
                    "Power": 10,
                    "SimulationId": "2023-02-23T08:31:20.924Z",
                    "SourceProcessId": "ic1",
                    "StationId": "3",
                    "Timestamp": "2023-02-23T08:31:37.608000Z",
                    "Topic": "PowerRequirementTopic",
                    "TriggeringMessageIds": [
                        "SimulationManager-2"
                    ],
                    "Type": "PowerRequirement",
                    "UserId": 4
                },
                {
                    "EpochNumber": 1,
                    "MessageId": "ic1-5",
                    "Power": 0,
                    "SimulationId": "2023-02-23T08:31:20.924Z",
                    "SourceProcessId": "ic1",
                    "StationId": "2",
                    "Timestamp": "2023-02-23T08:31:37.610000Z",
                    "Topic": "PowerRequirementTopic",
                    "TriggeringMessageIds": [
                        "SimulationManager-2"
                    ],
                    "Type": "PowerRequirement",
                    "UserId": 2
                },
                {
                    "EpochNumber": 1,
                    "MessageId": "ic1-6",
                    "SimulationId": "2023-02-23T08:31:20.924Z",
                    "SourceProcessId": "ic1",
                    "Timestamp": "2023-02-23T08:31:37.621000Z",
                    "Topic": "Status.Ready",
                    "TriggeringMessageIds": [
                        "SimulationManager-2"
                    ],
                    "Type": "Status",
                    "Value": "ready"
                }
            ]

- Get a list of messages with topic name of PowerOutputTopic at epoch 2.

    - Request:

        [`http://localhost:8080/simulations/<simulation_id>/messages?topic=PowerOutputTopic&epoch=2`](http://localhost:8080/simulations/<simulation_id>/messages?topic=PowerOutputTopic&epoch=2)

    - Response:

            :::json
            [
                {
                    "EpochNumber": 2,
                    "MessageId": "s1-6",
                    "PowerOutput": 20,
                    "SimulationId": "2023-02-23T08:31:20.924Z",
                    "SourceProcessId": "s1",
                    "StationId": "1",
                    "Timestamp": "2023-02-23T08:31:37.639000Z",
                    "Topic": "PowerOutputTopic",
                    "TriggeringMessageIds": [
                        "SimulationManager-3"
                    ],
                    "Type": "PowerOutput",
                    "UserId": 1
                },
                {
                    "EpochNumber": 2,
                    "MessageId": "s3-6",
                    "PowerOutput": 10,
                    "SimulationId": "2023-02-23T08:31:20.924Z",
                    "SourceProcessId": "s3",
                    "StationId": "3",
                    "Timestamp": "2023-02-23T08:31:37.640000Z",
                    "Topic": "PowerOutputTopic",
                    "TriggeringMessageIds": [
                        "SimulationManager-3"
                    ],
                    "Type": "PowerOutput",
                    "UserId": 4
                },
                {
                    "EpochNumber": 2,
                    "MessageId": "s2-6",
                    "PowerOutput": 0,
                    "SimulationId": "2023-02-23T08:31:20.924Z",
                    "SourceProcessId": "s2",
                    "StationId": "2",
                    "Timestamp": "2023-02-23T08:31:37.641000Z",
                    "Topic": "PowerOutputTopic",
                    "TriggeringMessageIds": [
                        "SimulationManager-3"
                    ],
                    "Type": "PowerOutput",
                    "UserId": 2
                }
            ]

- Get a list of messages with topic name of PowerOutputTopic for station 2 component (s2) between epoch 3 and epoch 5.

    - Request:

        [`http://localhost:8080/simulations/<simulation_id>/messages?topic=PowerOutputTopic&process=s2&startEpoch=3&endEpoch=5`](http://localhost:8080/simulations/<simulation_id>/messages?topic=PowerOutputTopic&process=s2&startEpoch=3&endEpoch=5)

    - Response:

            :::json
                [
                    {
                        "EpochNumber": 3,
                        "MessageId": "s2-9",
                        "PowerOutput": 12,
                        "SimulationId": "2023-02-23T08:31:20.924Z",
                        "SourceProcessId": "s2",
                        "StationId": "2",
                        "Timestamp": "2023-02-23T08:31:37.670000Z",
                        "Topic": "PowerOutputTopic",
                        "TriggeringMessageIds": [
                            "SimulationManager-4"
                        ],
                        "Type": "PowerOutput",
                        "UserId": 2
                    },
                    {
                        "EpochNumber": 4,
                        "MessageId": "s2-12",
                        "PowerOutput": 12,
                        "SimulationId": "2023-02-23T08:31:20.924Z",
                        "SourceProcessId": "s2",
                        "StationId": "2",
                        "Timestamp": "2023-02-23T08:31:37.701000Z",
                        "Topic": "PowerOutputTopic",
                        "TriggeringMessageIds": [
                            "SimulationManager-5"
                        ],
                        "Type": "PowerOutput",
                        "UserId": 2
                    },
                    {
                        "EpochNumber": 5,
                        "MessageId": "s2-15",
                        "PowerOutput": 12,
                        "SimulationId": "2023-02-23T08:31:20.924Z",
                        "SourceProcessId": "s2",
                        "StationId": "2",
                        "Timestamp": "2023-02-23T08:31:37.727000Z",
                        "Topic": "PowerOutputTopic",
                        "TriggeringMessageIds": [
                            "SimulationManager-6"
                        ],
                        "Type": "PowerOutput",
                        "UserId": 2
                    }
                ]

## Running the simulation and checking the graphs on remote server

The simulation can be run using an application that is hosted on a server. Simulation output can be displayed as graphs in the application. The application can be accessed using the following link: [`https://evc.tlt-cityiot.rd.tuni.fi/`](https://evc.tlt-cityiot.rd.tuni.fi/). The application portal requires user authentication.


## Running the simulation and checking the graphs locally

The application that is hosted on the server can also be run locally. Instructions to run the application locally can be found from the following link: [`https://github.com/EVCommunities/GUI`](https://github.com/EVCommunities/GUI).