# Following a running simulation

There are a couple of ways to follow a running simulation that has been started using the simulation platform. Commands for the first two ways are given by Platform Manager after the simulation has been started.

- All the component outputs can be followed using a Bash script `follow_simulation.sh`.
- The output from any participating simulation component can be followed using `docker logs` command.
- The logged messages from the simulation can be looked at using Log Reader even during the simulation. See the [Log Reader API](core_log-api.md) for more information.

The platform managed Docker containers use a name format `Sim<id_number>_<component_name>` where `<id_number>` is the first available 2-digit number (00, 01, ..., 99) at the start of the simulation and `<component_name>` is the component name used in the simulation run. For example, for the first started simulation, the Simulation Manager container should be named `Sim00_SimulationManager`. The Platform Manager outputs the `<id_number>` for the started simulation run.

## Using follow simulation script

To start following the running simulation with colored output from all the platform managed components, using Bash compatible terminal navigate to the installation folder and use the command:

    :::bash
    source follow_simulation.sh <id_number>

where `<id_number>` is given by the Platform Manager after the simulation run has been started. The full command is also part of the Platform Manager output.

Note, that due to the way this follow script has been implemented, it cannot be easily cancelled and if you want to stop following the simulation the easiest way to do so is to close the terminal window. Closing the terminal window while a simulation is running does not affect the actual simulation.

## Using docker logs command

To follow the advancing of the simulation, the output from any of the participating components can be followed using the command (can be used from any folder and does not require Bash compatible terminal, e.g. works also with Command Prompt in Windows):

    :::bash
    docker logs --follow Sim<id_number>_<component_name>

where `<id_number>` is given by the Platform Manager after the simulation run has been started and `<component_name>` is the name of the component as it is defined in the simulation configuration file. The full command to follow the output from the Simulation Manager is also part of the Platform Manager output.

Following the log output can be cancelled using the key combination `Ctrl+C`. Cancelling the log following does not affect the actual simulation.

## Fetching log files after a simulation run

The platform managed Docker containers are automatically deleted after the simulation is finished, and thus the component outputs cannot be looked at using the follow simulation script or docker logs command after the simulation is finished.

After the simulation, the output from the components can be found stored in log files in a Docker volume called `simces_simulation_logs` in the folder `/logs`. The log files can be fetched to a local folder using a Bash script `logs/copy_logs.sh`:

1. Navigate to the folder logs inside the installation folder using a Bash compatible terminal.
2. Use the following command to copy all the log files to the logs folder:

        :::bash
        source copy_logs.sh

The filenames for the log files use the format `logfile_<component_name>.log`. Note that the files can contain outputs from multiple simulations. The latest outputs are at the end of the log files.
