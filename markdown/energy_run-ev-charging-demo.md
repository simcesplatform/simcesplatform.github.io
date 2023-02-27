# Running the EV Charging demo scenario simulation

## Installing the core component

Instructions for installing core components: [Instructions for installing core compoenents](core_install.md)


## Installing the domain component

To be able to use the [User](energy_user-component.md), [Station](energy_station-component.md), [Intelligent Controller](energy_ic-component.md) component in a simulation run, it must first be installed. Use the following steps to make it available for the platform to use.

1. Ensure that the file `docker_images_domain.txt` contains the Docker image name for the following components:
    - UserComponent: `ghcr.io/evcommunities/user-component:latest` 
    - StationComponent: `ghcr.io/evcommunities/station-component:latest`
    - ICComponent: `ghcr.io/evcommunities/ic-component:latest`
    
2. Ensure that the file `components/github_server_domain.yml` contains in the `Repositories` list the code repository for User, Station and IC: `EVCommunities/Components`

3. Using Bash compatible terminal (Git Bash in Windows) navigate to the `platform` folder (the folder where the platform is installed).

4. Install components along with the other domain components by using the command:

        :::bash
        source platform_domain_setup.sh

    This script will fetch the Docker images and component manifest files for the domain components as well as make all the input files in the `resources` folder available for the simulation platform.

5. (Optional) Look through the simulation configuration file and see how it corresponds to the scenario description at [EV charging demo scenario](energy_scenario-ev-charging-demo.md). The ready-made configuration file can be found at [simulation_configuration_ev_charging.yml](https://github.com/simcesplatform/Platform-Manager/blob/master/simulation_configuration_ev_charging.yml).

## Starting the simulation run

To start the EV charging demo scenario simulation, use Bash compatible terminal to navigate to the `platform` folder and use the command

    :::bash
    source start_simulation.sh simulation_configuration_ev_charging.yml

To follow the simulation run while it is running see the page: [Following a running simulation](core_follow-run.md)

## Checking the results from the simulation run

After the simulation run has been completed you should be able to use the [Log Reader](core_log-api.md) to look through the messages used in the simulation. A couple of examples of using the Log Reader API are given below. In all examples, `<simulation_id>` should be replaced by the simulation id given by the Platform Manager.

Note, that when using the Log Reader API, the special `#` characters which can be used with RabbitMQ topic names, must be replaced with `%23` as has been done in the last example. See more information about URL encoding from [https://www.w3schools.com/tags/ref_urlencode.ASP](https://www.w3schools.com/tags/ref_urlencode.ASP).
