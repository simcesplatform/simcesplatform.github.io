# Stopping a running simulation

After a successful simulation all the components that participated in the simulation should be closed automatically. However, if there were some errors during the simulation run, some components might be left running indefinitely unless they are manually stopped.

To stop a running simulation by closing all the platform managed containers, using Bash compatible terminal navigate to the installation folder, by default this is `platform`, and use the command:

    :::bash
    source stop_simulation.sh <id_number>

where `<id_number>` is given by the simulation platform after the simulation was started.

You can use command

    :::bash
    docker ps --filter name=Sim --format "{{.Names}}"

to see all the currently running containers started by the simulation platform.

Note, that this will only stop the platform managed components and does not affect the externally managed components in any way.
