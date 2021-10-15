# Running the Energy Community demo scenario simulation

After successfully [running the first test simulation](core_run-first.md), the next step is to run a simulation with at least one domain component. The Energy Community (EC) demo scenario uses platform-managed [StaticTimeSeriesResource](energy_static-time-series-resource.md) components along with the core components. The scenario is defined at the documentation page [Energy community demo scenario](energy_scenario-ec-demo.md).

## Installing the domain component

To be able to use the [StaticTimeSeriesResource](energy_static-time-series-resource.md) component in a simulation run, it must first be installed. Use the following steps to make it available for the platform to use.

1. Ensure that the file `docker_images_domain.txt` contains the Docker image name for the StaticTimeSeriesResource: `ghcr.io/simcesplatform/static-time-series-resource:latest`

2. Ensure that the file `components/github_server_domain.yml` contains in the `Repositories` list the code repository for StaticTimeSeriesResource: `simcesplatform/static-time-series-resource`

3. Using Bash compatible terminal (Git Bash in Windows) navigate to the `platform` folder (the folder where the platform is installed).

4. Install the StaticTimeSeriesResource component along with the other domain components by using the command:

        :::bash
        source platform_domain_setup.sh

    This script will fetch the Docker images and component manifest files for the domain components as well as make all the input files in the `resources` folder available for the simulation platform.

5. (Optional) Look through the simulation configuration file and see how it corresponds to the scenario description at Energy Community (EC). The ready-made configuration file can be found at [simulation_configuration_ec.yml](https://github.com/simcesplatform/Platform-Manager/blob/master/simulation_configuration_ec.yml). Note, that the filenames for the CSV files that resource component uses are given as `/resources/<filename>` where the `<filename>` is the corresponding filename at the local folder `platform/resources/`

## Starting the simulation run

To start the EC demo scenario simulation, use Bash compatible terminal to navigate to the `platform` folder and use the command

    :::bash
    source start_simulation.sh simulation_configuration_ec.yml

To follow the simulation run while it is running see the page: [Following a running simulation](core_follow-run.md)

## Checking the results from the simulation run

After the simulation run has been completed you should be able to use the [Log Reader](core_log-api.md) to look through the messages used in the simulation. A couple of examples of using the Log Reader API are given below. In all examples, `<simulation_id>` should be replaced by the simulation id given by the Platform Manager.

- Get a list of messages sent from the electric vehicle component (electric_vehicle) at epoch 16.

    - Request:

        [http://localhost:8080/simulations/<simulation_id>/messages?process=electric_vehicle&epoch=12](http://localhost:8080/simulations/<simulation_id>/messages?process=electric_vehicle&epoch=12)

    - Response:

            :::json
            [
                {
                    "CustomerId": "10",
                    "EpochNumber": 16,
                    "MessageId": "electric_vehicle-32",
                    "Node": 2,
                    "ReactivePower": {
                        "UnitOfMeasure": "kV.A{r}",
                        "Value": 0.0
                    },
                    "RealPower": {
                        "UnitOfMeasure": "kW",
                        "Value": -5.5
                    },
                    "SimulationId": "2021-10-12T13:37:48.914Z",
                    "SourceProcessId": "electric_vehicle",
                    "Timestamp": "2021-10-12T13:38:53.593000Z",
                    "Topic": "ResourceState.Load.electric_vehicle",
                    "TriggeringMessageIds": [
                        "simulation-manager-17"
                    ],
                    "Type": "ResourceState"
                },
                {
                    "EpochNumber": 16,
                    "MessageId": "electric_vehicle-33",
                    "SimulationId": "2021-10-12T13:37:48.914Z",
                    "SourceProcessId": "electric_vehicle",
                    "Timestamp": "2021-10-12T13:38:53.599000Z",
                    "Topic": "Status.Ready",
                    "TriggeringMessageIds": [
                        "simulation-manager-17"
                    ],
                    "Type": "Status",
                    "Value": "ready"
                }
            ]

- Get a time series for the real powers for the first load (load_1) for the entire simulation (the result given in CSV format).

    - Request:

        [`http://localhost:8080/simulations/<simulation_id>/timeseries?attrs=RealPower&topic=ResourceState.Load.load_1&format=csv`](http://localhost:8080/simulations/<simulation_id>/timeseries?attrs=RealPower&topic=ResourceState.Load.load_1&format=csv)

    - Response:

            :::text
            epoch;timestamp;ResourceState.Load.load_1:load_1.RealPower
            1;2020-06-24T21:00:00Z;-0.2
            2;2020-06-24T22:00:00Z;-0.27
            3;2020-06-24T23:00:00Z;-0.15
            4;2020-06-25T00:00:00Z;-0.21
            5;2020-06-25T01:00:00Z;-0.26
            6;2020-06-25T02:00:00Z;-0.15
            7;2020-06-25T03:00:00Z;-0.21
            8;2020-06-25T04:00:00Z;-0.26
            9;2020-06-25T05:00:00Z;-0.16
            10;2020-06-25T06:00:00Z;-0.22
            11;2020-06-25T07:00:00Z;-0.23
            12;2020-06-25T08:00:00Z;-0.17
            13;2020-06-25T09:00:00Z;-0.24
            14;2020-06-25T10:00:00Z;-0.2
            15;2020-06-25T11:00:00Z;-0.17
            16;2020-06-25T12:00:00Z;-0.27
            17;2020-06-25T13:00:00Z;-0.18
            18;2020-06-25T14:00:00Z;-0.18
            19;2020-06-25T15:00:00Z;-0.28
            20;2020-06-25T16:00:00Z;-0.16
            21;2020-06-25T17:00:00Z;-0.2
            22;2020-06-25T18:00:00Z;-0.27
            23;2020-06-25T19:00:00Z;-0.15
            24;2020-06-25T20:00:00Z;-0.21

- Get a time series for real powers of the generators between epochs 5 and 8 (the result given in CSV format).

    - Request:

        [`http://localhost:8080/simulations/<simulation_id>/timeseries?attrs=RealPower&topic=ResourceState.Generator.%23&startEpoch=5&endEpoch=8&format=csv`](http://localhost:8080/simulations/<simulation_id>/timeseries?attrs=RealPower&topic=ResourceState.Generator.%23&startEpoch=5&endEpoch=8&format=csv)

    - Response:

            :::text
            epoch;timestamp;ResourceState.Generator.pv_large:pv_large.RealPower;ResourceState.Generator.pv_small:pv_small.RealPower
            5;2020-06-25T01:00:00Z;0.35;0.14
            6;2020-06-25T02:00:00Z;0.84;0.34
            7;2020-06-25T03:00:00Z;1.4;0.57
            8;2020-06-25T04:00:00Z;1.89;0.76

Note, that when using the Log Reader API, the special `#` characters which can be used with RabbitMQ topic names, must be replaced with `%23` as has been done in the last example. See more information about URL encoding from [https://www.w3schools.com/tags/ref_urlencode.ASP](https://www.w3schools.com/tags/ref_urlencode.ASP).
