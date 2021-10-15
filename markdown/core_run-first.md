# Running the first test simulation

To test that the core platform has been installed properly, a simple test simulation configuration has been provided. This configuration involves 3 [Dummy components](core_dummycomponent.md) in addition to the [Simulation Manager](core_simulationmanager.md) and [Log Writer](core_logwriter.md) instances that are always included in every simulation run.

## (Optional) Configuring the platform settings

The default parameters are fine for testing the platform installation. By default the local RabbitMQ message bus and the local MongoDB that were started during the installation process are used.

The descriptions for the configurable settings can be found from [Configuring platform settings](core_platform-settings.md).

## (Optional) Setting up the simulation configuration file

The simulation run specific parameters are given in a separate configuration file that uses [YAML](https://yaml.org/) format. For the first test run, a ready-made configuration file, [simulation_configuration_test.yml](https://github.com/simcesplatform/platform-manager/blob/master/simulation_configuration_test.yml) has been made. The configuration file defines

- the simulation metadata: the name and description for the simulation run as well as the start time for the first epoch, epoch length and the maximum number of epochs in the simulation run
- the components participating in the simulation run and their individual parameters

For the first test simulation, the configuration file can be left as it is.

## Starting the first simulation run

To start the first test simulation, use Bash compatible terminal (Git Bash in Windows) to navigate to the platform folder and use the command

    :::bash
    source start_simulation.sh simulation_configuration_test.yml

If everything worked properly, you should see something like:

    :::text
    simces_platform_manager | 2021-10-12T12:50:00.515 ---     INFO --- Starting the Docker containers for simulation: 'Test simulation' with id: 2021-10-12T12:50:00.514Z
    simces_platform_manager | 2021-10-12T12:50:00.878 ---     INFO --- Starting container: Sim00_log_writer
    simces_platform_manager | 2021-10-12T12:50:01.376 ---     INFO --- Starting container: Sim00_slow_dummy
    simces_platform_manager | 2021-10-12T12:50:01.815 ---     INFO --- Starting container: Sim00_fast_dummy_1
    simces_platform_manager | 2021-10-12T12:50:02.240 ---     INFO --- Starting container: Sim00_fast_dummy_2
    simces_platform_manager | 2021-10-12T12:50:02.671 ---     INFO --- Starting container: Sim00_SimulationManager
    simces_platform_manager | 2021-10-12T12:50:03.095 ---     INFO --- Start message for simulation 'Test simulation' sent to management exchange.
    simces_platform_manager | 2021-10-12T12:50:03.095 ---     INFO --- Simulation 'Test simulation' started successfully using id: 2021-10-12T12:50:00.514Z
    simces_platform_manager | 2021-10-12T12:50:03.095 ---     INFO --- Follow the simulation by using the command:
    simces_platform_manager |     source follow_simulation.sh 00
    simces_platform_manager | 2021-10-12T12:50:03.095 ---     INFO --- Alternatively, the simulation manager logs can by viewed by:
    simces_platform_manager |     docker logs --follow Sim00_SimulationManager
    simces_platform_manager | 2021-10-12T12:50:03.095 ---     INFO --- Platform manager has finished starting the simulation and will now stop.
    simces_platform_manager | 2021-10-12T12:50:03.095 ---     INFO --- The simulation will continue to run on the background.
    simces_platform_manager | 2021-10-12T12:50:03.095 ---     INFO --- Stopping the platform manager.
    simces_platform_manager exited with code 0

See the [Following a running simulation](core_follow-run.md) page on more details about following the output from the running simulation.

The [Log Reader](core_log-api.md), by default at [http://localhost:8080](http://localhost:8080) can also be used to view the messages.

- [`http://localhost:8080/simulations/<simulation_id>`](http://localhost:8080/simulations/<simulation_id>) should show the metadata for the test simulation
- [`http://localhost:8080/simulations/<simulation_id>/messages`](http://localhost:8080/simulations/<simulation_id>/messages) should show all the messages for the test simulation in the order of their arrival
- [`http://localhost:8080/simulations/<simulation_id>/messages?topic=Start`](http://localhost:8080/simulations/<simulation_id>/messages?topic=Start) should show only the start message for the test simulation

In the above, replace `<simulation_id>` with the id given by the Platform manager. In the earlier output, the simulation id would be `2021-10-12T12:50:00.514Z`. See the [Log Reader API documentation](core_log-api.md) page for more details about the using the API.
