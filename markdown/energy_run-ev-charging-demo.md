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

5. Copy the ready-made simulation configuration file from [simulation_configuration_ev_charging.yml](https://github.com/simcesplatform/Platform-Manager/blob/master/simulation_configuration_ev_charging.yml) to the `platform` folder.
   a. Either copy the file manually or use the following command line instruction:

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

- `TODO`: add Log reader examples for an user to be able to confirm that the scenario has been run correctly
- `TODO`: add a link to the EVCommunities GUI demo page
