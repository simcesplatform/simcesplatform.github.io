# Preparations for a simulation

All file and folder names are given as relative to the platform installation folder, i.e. the folder where the file `fetch_platform_files.sh` is located at.

There are a couple steps before a simulation run using user developed components can be be started using the platform:

- All the components have to be installed. ([Installing a new component](core_run-preparations.md#installing-a-new-component))
    - With externally managed components this involves installing and starting the components manually.
    - With platform managed components, this involves fetching the Docker image for the component.
- All the components have to be registered to the platform. ([Registering new component type to the platform](core_run-preparations.md#registering-component-type-to-the-platform)) by providing a component manifest file for the component.
- For platform managed components any input files (for example, CSV files) have to be made available to the platform. ([Making input files available for the platform](core_run-preparations.md#making-input-files-available-for-the-platform))

After the component installation and registration has been done, a new simulation configuration file can be created and then used to start a new simulation run. See page [Starting a general simulation](core_start-simulation.md) for instructions regarding starting a simulation.

## Installing a new component

### Externally managed components

Any externally managed component should contain its own installation instructions on the documentation page of the component.

### Platform managed components

A component managed by the platform should have an available Docker image that can be used to include the component in simulation runs. This Docker image must be available to the platform when a new simulation run that includes this component is started.

To fetch the Docker image for the component follow the procedure:

1. Modify the file `docker_images_domain.txt` so that it contains the Docker image name for the component in question.
    - Note, that the file must be saved using Unix line endings (LF)
        - To change the file from Windows line endings (CR LF) to Unix line endings with Notepad++: `Edit -> EOL Conversion -> Unix (LF)`
        - Or using Bash compatible terminal: `dos2unix docker_images_domain.txt`

2. Once the component registration and the input files have also been setup, follow the instructions at ([Finalize component installation](core_run-preparations.md#finalize-component-installation)) to finalize the component installation.

### Downloading component source code

The full source should not be needed when installing components, at least not with the platform managed components, but the following is included here if it is required.

The general form of the command to fetch the source code using Git is:

```bash
git clone --recursive https://github.com/simcesplatform/<component_name>.git
```

or if the code is in a private GitLab repository (with an insecure SSL certificate at the host):

```bash
git -c http.sslVerify=false clone --recursive <full_address_to_the_repository>.git
```

## Registering component type to the platform

Before platform can be used to start a new simulation that includes a new domain component, the new component type has to be registered to the platform. The registering is done to allow the platform to know whether the new component is an externally managed or platform managed component and in the latter case to also know which Docker image to use when deploying the platform-managed component. The registration process also allows the user to mark some input parameters as required so that the platform does not start a new simulation run in vain if essential information is missing from the simulation configuration.

The components are registered for the platform by providing it with component manifest files for all the required component types.

Each component should provide a component manifest file with the component source code.

To register all the required component types to the platform follow the procedure:

1. Modify the Git server file so that it contains the code repository name for the component in question in the Repositories listing. See the section Git server file format about the details of the used attributes and the syntax for the file.
    - Use the file `components/github_server_domain.yml` if the repositories are available at GitHub
    - Use the file `components/gitlab_server_domain.yml` if the repositories are in the GitLab server used in the development of the platform.
    - It is also possible to create completely new Git server file under the folder `components`. All files under the folder are gone through when fetching the manifest files.
    - Alternatively, it is also possible to manually copy the component manifest file to the folder `manifests`. All YAML files under that folder are taken to account when the platform is started.
    - There also exists a script that can be used to copy locally existing manifest files to the manifests folder. To use it, first edit the file `local_manifest_files.txt` to include the paths to the local manifest files and then run: `source fetch_local_manifests.sh local_manifest_files.txt`
2. Once the component installation as well as the input files have been setup, follow the instructions at ([Finalize component installation](core_run-preparations.md#finalize-component-installation)) to finalize the component installation.

### Git server file format

The server file is in YAML format and supports the following attributes:

- `Type` (required)
    - can be either "`GitLab`" or "`GitHub`" depending on the remote Git server type
- `Repositories` (required)
    - a list of repository names from which a component manifest should be fetched
    - each repository can contain the following attributes:
        - `File` (optional)
            - the filename for the component manifest file, must have an extension of "`.yml`" or "`.yaml`"
            - the default filename is "`component_manifest.yml`"
        - `Branch` (optional)
            - the branch or tag of the repository that should be used when fetching the manifest file
            - the default branch is "`master`"
- `Host` (optional)
    - the host server address
    - only taken account for GitLab servers, for GitHub server, "`https://github.com`", is used
    - the default value for GitLab server is "`https://gitlab.com`"
- `Certificate` (optional)
    - whether the host server has a proper SSL certificate
    - can be either `true` or `false`, the default value is `true`
- `AccessToken` (optional)
    - An access token to be used when fetching files from a private repository
    - If given in the form "`${ENV_VARIABLE}`" the value of the environmental variable `ENV_VARIABLE` is used as the access token. The platform loads the environment variables from file `access_tokens.env` when fetching the manifest files.
    - The given access token must provide at least read access to the given repositories
    - If no access token is given, all repositories are assumed to be public

### Example Git server file

The following server file would be used to fetch manifest files from 4 GitLab repositories hosted at `https://gitlab.address.com`. Using the default branch, file "`component_manifest.yml`" from "`procemplus/my_component1`" and "`procemplus/my_component2`" and file "`my_manifest.yml`" from "`procemplus/my_component3`". Using the branch "`my_branch`", file "`my_folder/my_manifest.yml`" from "`username/my_component4`". The access to the repositories is provided by environment variable `GITLAB_ACCESS_TOKEN`.

```yaml
Repositories:
    - procemplus/my_component1
    - procemplus/my_component2
    - procemplus/my_component3:
        File: my_manifest.yml
    - username/my_component4:
        File: my_folder/my_manifest.yml
        Branch: my_branch
Type: GitLab
Host: https://gitlab.address.com
Certificate: false
AccessToken: ${GITLAB_ACCESS_TOKEN}
```

## Making input files available for the platform

Any input file that is used by a platform managed deployed component during a simulation run, needs to be made available for the platform. Example for an input file is the CSV file that is used as input with the [Static Time Series Resource](energy_static-time-series-resource.md) components in the [EC demo simulation](energy_run-ec-demo.md).

Any input files that are used by externally managed components are not required to be accessible by the platform.

To make a input file available for the platform:

1. Copy the input file to the `resources` folder. The files can be either directly at the resources folder (like the input files for EC demo scenario) or inside a subfolder.

2. Once the component installation and registration have also been setup, follow the instructions at ([Finalize component installation](core_run-preparations.md#finalize-component-installation)) to finalize the component installation.

Note, that due to the limitation of the current platform implementation, all the files included in the `resources` folder will be made available for all the platform managed components. Also, to avoid errors, any file that is currently being used in a running simulation should not be modified while the simulation is still running.

## Finalize component installation

The component can be installed and registered to the platform by running the following installation script. This script will also make any prepared input files available to the platform.

```bash
source platform_domain_setup.sh
```

A successful run of the script should show no error messages in the output.

Note, that for externally managed components, the script above can only register the component manifests to the platform. The manual installation and starting procedure for the components is still required in addition to running the script.

### Example output

```text
Fetching the domain Docker images from Docker registry
Reading 'docker_images_domain.txt' for Docker image names
Pulling Docker image: ghcr.io/simcesplatform/static-time-series-resource:latest
latest: Pulling from simcesplatform/static-time-series-resource
Digest: sha256:844b269933eb9272bed896d83e9ace4d0b4ad9311507a3027c12a874f3c7087c
Status: Image is up to date for ghcr.io/simcesplatform/static-time-series-resource:latest
ghcr.io/simcesplatform/static-time-series-resource:latest

Fetching the component manifests.
Creating simces_manifest_fetcher ... done
Attaching to simces_manifest_fetcher
simces_manifest_fetcher | 2021-10-14T08:04:48.932 ---     INFO --- Fetching file 'component_manifest.yml' from GitHub repository simcesplatform/static-time-series-resource
simces_manifest_fetcher | 2021-10-14T08:04:49.217 ---     INFO --- Fetching file 'component_manifest_manager.yml' from GitHub repository simcesplatform/simulation-manager
simces_manifest_fetcher | 2021-10-14T08:04:49.417 ---     INFO --- Fetching file 'component_manifest_dummy.yml' from GitHub repository simcesplatform/simulation-manager
simces_manifest_fetcher | 2021-10-14T08:04:49.629 ---     INFO --- Fetching file 'component_manifest.yml' from GitHub repository simcesplatform/logwriter
simces_manifest_fetcher | 2021-10-14T08:04:49.820 ---     INFO --- Fetching file 'component_manifest_manager.yml' from GitHub repository simcesplatform/simulation-manager
simces_manifest_fetcher | 2021-10-14T08:04:49.847 ---     INFO --- Fetching file 'component_manifest_dummy.yml' from GitHub repository simcesplatform/simulation-manager
simces_manifest_fetcher | 2021-10-14T08:04:49.865 ---     INFO --- Fetching file 'component_manifest.yml' from GitHub repository simcesplatform/logwriter
simces_manifest_fetcher exited with code 0
Going to remove simces_manifest_fetcher
Removing simces_manifest_fetcher ... done

Copying the contents of 'resources' to Docker volume 'simces_simulation_resources' to folder '/resources'
Copying file resources/Load2.csv
Copying file resources/Load1.csv
Copying file resources/Load4.csv
Copying file resources/PV_large.csv
Copying file resources/Load3.csv
Copying file resources/PV_small.csv
Copying file resources/EV.csv
```
