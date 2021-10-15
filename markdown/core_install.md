# Installation

Follow the given steps to install the platform.

## Prerequisites

The simulation platform and these instructions have been tested on Ubuntu 18.04 with Docker Engine version 20.10.2 and Docker Compose version 1.28.2.

- [Bash](https://www.gnu.org/software/bash/)
    - For running the helper scripts.
    - The "curl" command must also be installed (usually available by default).
    - For Windows, Bash is included with the [Git for Windows](https://git-scm.com/downloads), for other operating systems it is likely available by default.
    - On Windows the [Windows Subsystem for Linux (WSL)](https://docs.microsoft.com/en-us/windows/wsl/install-win10) can also be used, though setting it up requires more technical knowledge than installing Git for Windows.
- [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/install/)
    - For running platform-managed components for the simulations.
    - For running the local RabbitMQ message bus as well as the local MongoDB database. These are installed by the installation scripts.
    - Installing the most up-to-date versions of Docker and Docker Compose is the recommended option.
    - On Linux, to be compatible with the platform scripts it is advisable to set Docker to be manageable as a non-root user: [https://docs.docker.com/engine/install/linux-postinstall/](https://docs.docker.com/engine/install/linux-postinstall/)

Some simulation components might have other requirements. For those, see the component specific documentation.

## Installing the platform and the core components

- 1\. Create a folder for the simulation platform. In these instructions this main folder is named platform.
    - Note, that you must have full read and write permissions in the created folder, platform. Otherwise, the installation process will not work.

- 2\. Copy the [`fetch_platform_files.sh`](https://github.com/simcesplatform/Platform-Manager/blob/master/fetch_platform_files.sh) script file to the `platform` folder for by downloading it from the GitHub using the browser interface (right click on the "Raw" button and "Save Link As...") or by using the command line:

        :::bash
        wget https://raw.githubusercontent.com/simcesplatform/Platform-Manager/master/fetch_platform_files.sh

- 3\. Using Git Bash (in Windows) or other terminal that supports Bash navigate to the platform folder.

    - In the terminal you can navigate inside a folder using command: `cd <folder_name>`
          - If the folder name contains a whitespace character, quotes must be used around the folder name. For example, to navigate to folder "My Folder", use: `cd "My Folder"`
    - To navigate to the parent folder, use command: `cd ..`
    - To check your current folder, use command: `pwd`
    - To list the files and sub folders in the current folder, use command: `ls -l`

- 4\. Run the fetch script using the command

        :::bash
        source fetch_platform_files.sh github

    Answer "y" when prompted to start fetching the source code.

    If everything went alright, the script should print the following line at the end:

        :::text
        All files where found. Platform is ready for installation.

- 5\. (Optional) Setup the settings for the RabbitMQ message bus and the Mongo database. Note, that the default settings can be used for running simulations locally.

    - See section [General platform level settings](core_platform-settings.md#general-platform-level-settings) on the available settings.

- 6\. Start the simulation platform by running the following command in the platform folder using a terminal that supports Bash

        :::bash
        source platform_core_setup.sh

    The script will fetch all the platform components (their Docker images) and other files that are needed to run the first simulations. After the fetching stage the script will then start all the required background processes. Running the script will take a few minutes.

- 7\. (Optional) Check that all the required platform components have been downloaded.

    To list the built Docker images run the command

        :::bash
        docker images

    You should have at least the images shown in the example listing below:

        :::text
        REPOSITORY                                      TAG                IMAGE ID       CREATED        SIZE
        ghcr.io/simcesplatform/platform-manager         latest             1330807ae5cc   9 days ago     909MB
        ghcr.io/simcesplatform/simulation-manager       latest             d14c1a9bfbd0   9 days ago     896MB
        ghcr.io/simcesplatform/manifest-fetcher         latest             3486b5fac52a   9 days ago     903MB
        ghcr.io/simcesplatform/logwriter                latest             d9f1d537169b   9 days ago     900MB
        ghcr.io/simcesplatform/dummy-component          latest             76811eb2d295   10 days ago    1.12GB
        ghcr.io/simcesplatform/mongo-express            latest             f43423478d66   3 months ago   127MB
        ghcr.io/simcesplatform/logreader                latest             42d8ca7ecd53   3 months ago   956MB
        rabbitmq                                        3.8.4-management   cc86ffa2f398   9 months ago   186MB
        mongo                                           4.2.7              66c68b650ad4   9 months ago   388MB

- 8\. (Optional) Check that all the background components have been started.

    - 8\.1\.To list all the running Docker containers run the command

            :::bash
            docker ps --format "table {{.ID}}\t{{.Image}}\t{{.Status}}\t{{.Names}}"

        You should have at least those containers running that are shown in the example listing below:

            :::text
            CONTAINER ID   IMAGE                                   STATUS         NAMES
            c754c5009a28   mongo-express:0.54.0                    Up 2 minutes   simces_mongo_express
            fd6e87195259   procem.ain.rd.tut.fi/despa/log_reader   Up 2 minutes   simces_log_reader
            f33e24b1d060   procem.ain.rd.tut.fi/despa/log_writer   Up 2 minutes   simces_log_writer_management
            9782e68eabbc   rabbitmq:3.8.4-management               Up 2 minutes   simces_rabbitmq
            fa0a61528313   mongo:4.2.7                             Up 2 minutes   simces_mongodb

    - 8\.2\. Check that the Log Writer instance listening to the management exchange is running properly. This also works as a check for the RabbitMQ message bus.

        - To check the log output from the Log Writer instance use the command

                :::bash
                docker logs simces_log_writer_management

        - If you see `Now listening to messages; exc=procem-management, topic=#` and there is no `Connect call failed` or `Closing listener for topics: '#'` after the `Now listening ...` line, the Log Writer instance should be running properly.
            - Note, that it does not matter if output contains connection fail messages before the last `Now listening ...` line as long as the there no such messages after that line.
        - If the previous check indicates that the Log Writer instance is not working properly, do the following:
            - Stop the Log Writer instance with commands:

                    :::bash
                    docker stop simces_log_writer_management
                    docker rm simces_log_writer_management

            - Optionally, check the parameters for the message bus (step 5).
            - Run the platform core setup script from step 6 again.
            - Check the Log Writer output again to see if the problem has been fixed.

    - 8\.3\. Check that the Log Reader is running. This also works as a check for the Mongo database.
        - The Log Reader should be running on localhost at port 8080. To check that it is responding use a browser to check that you can see the user interface page at the address [http://localhost:8080](http://localhost:8080).
        - For instructions on how to use the Log Reader see [Log API](core_log-api.md)
        - For advanced users
            - If you setup the Mongo Express, it can be accessed at the address [http://localhost:8081](http://localhost:8081).
                - Through the Mongo Express, you get admin access to the database by default, so care should be taken when making any changes to the database contents.
            - If you are using the local RabbitMQ instance, you can access the RabbitMQ Management Plugin at the address [http://localhost:15672](http://localhost:15672).

After these steps, the simulation platform should be ready for starting test simulation run. To see instructions on how to start a simulation run see the page [Running a simulation](core_run.md).

## Uninstalling the core components

The following commands are to be used in the installation folder using a terminal that supports Bash.

- To stop the running core components:

        :::bash
        docker-compose -f background/docker-compose-background.yml down --remove-orphans

- To remove the platform components entirely from the system (WARNING: this will remove all Docker images that are not in use, regardless of whether they are related to the simulation platform or not. However, the logged messages in the database from simulations will not be removed):

        :::bash
        docker system prune --all

- The files created in the installation folder have to be removed manually.
- For advanced users
    - For systems that have other Docker related things installed it is possibly not reasonable to run the general removal command. Use the following at your own discretion.
    - To stop and remove the background components:

            :::bash
            docker-compose -f background/docker-compose-background.yml down --remove-orphans

    - To stop and remove the Docker containers related to the platform (removes those containers that start with "simces" or "Sim"):

            :::bash
            docker ps --all --filter name="simces" --filter name="Sim" --format "{{.Names}}" | xargs docker stop
            docker ps --all --filter name="simces" --filter name="Sim" --format "{{.Names}}" | xargs docker rm

    - To remove the Docker images related to the platform:

            :::bash
            docker images -a | grep "procem.ain.rd.tut.fi\|mongo\|rabbitmq\|ubuntu" | awk '{print $3}' | xargs docker rmi

    - To remove the Docker networks related to the platform:

            :::bash
            docker network ls --filter name=simces --format "{{.Name}}" | xargs docker network rm

    - To also remove all the data related to the platform, i.e. to remove the Docker volumes related to the platform:

            :::bash
            docker volume ls --filter name=simces --format "{{.Name}}" | xargs docker volume rm
