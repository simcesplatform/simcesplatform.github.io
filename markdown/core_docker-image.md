# Building Docker image for a component

A component that is platform managed should have a Docker image that can be used by the platform to include in it in simulation runs.

Building a Docker image requires a Dockerfile that contains the machine instructions on how to start the component.

For a component developed in Python the template file [Dockerfile-template](https://github.com/simcesplatform/Platform-Manager/blob/master/instructions/Dockerfile-template) can be used as a starting point. It is based on the [Dockerfile](https://github.com/simcesplatform/static-time-series-resource/blob/master/Dockerfile) for the [Static Time Series Resource](energy_static-time-series-resource.md) component. The template file contains comment lines that should help creating a Dockerfile for a new component. The new Dockerfile should be placed at the root folder of the component source code. The recommended name for the file is "`Dockerfile`".

For the SimCES platform components, the [GitHub Container registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry) can be used to store the Docker images at the same place as the source code. See section [Using GitHub Container registry](#using-github-container-registry) for instructions on how to use it. Other Docker registries including Docker Hub and private registries can be used as well but no specific instructions for them are given.

The basic workflow is as follows:

- Use the `docker login` command to gain access to the Docker registry (only required for the first access). See section [Login to the container registry](#login-to-the-container-registry) for details.
- Create the `Dockerfile` for the component using the [template file](https://github.com/simcesplatform/Platform-Manager/blob/master/instructions/Dockerfile-template) as a starting point.
- Use the `docker build` command to build the Docker image. See section [Building Docker image](#building-docker-image) for details.
- Use the `docker push` command to push the created image to the registry. See section [Pushing image to container registry](#pushing-image-to-container-registry) for details.
    - (Optional) Make the image publicly available so that users can use the image without access credentials

Note, that the tag part in the Docker image name is used to differentiate between the different versions among the Docker images of the same name.

- To use the version `0.5` for a Docker image for a component called `my_component`, use the image name: `ghcr.io/simcesplatform/my_component:0.5`
- Using a Docker image name without the tag actually means that the default tag, `latest`, is used. I.e., in terms of Docker image names, `ghcr.io/simcesplatform/my_component` is the same as `ghcr.io/simcesplatform/my_component:latest`

## Using GitHub Container registry

When using the GitHub Container registry, the Docker image names for images linked to the SimCES platform will start with "`ghcr.io/simcesplatform`". For example, the image name for component called `my_component` could be "`ghcr.io/simcesplatform/my_component:latest`".

All of the following assumes that the user has a GitHub account and the required privileges in the `simcesplatform` GitHub organization.

### Login to the container registry

To be able to push Docker images to the container registry it is first required that the user logins to the registry. This is usually only required once for each user for each environment.

Steps required to login to the container registry:

1. Create a new personal access token (PAT) for your GitHub account by following instructions on page: [https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) (steps 1-9)
    - In the scope selection (step 8), at least "`write:packages`" must be selected to be able use the PAT to push images to the registry.
    - The newly created PAT must be stored somewhere private. It cannot be viewed after the creation.
2. Login to the Container registry
    - Use command: `docker login ghcr.io`
        - For username, use your GitHub account username
        - For password, use the PAT you just created
    - If the login was successful, you should see: "`Login Succeeded`"

### Building Docker image

To build a Docker image for the component from the source code using a name "`ghcr.io/simcesplatform/my_component:latest`" do the following:

- Run the following from the folder containing the `Dockerfile`:

        :::bash
        docker build --tag ghcr.io/simcesplatform/my_component:latest .

- If the used Dockerfile is name `Dockerfile-my-component` instead of `Dockerfile`, the following can be used:

        :::bash
        docker build --tag ghcr.io/simcesplatform/my_component:latest --file Dockerfile-my-component .

Note, that the Docker image name must match to the image name that was used in the component manifest file for the component.

### Pushing image to container registry

The following uses a Docker image `ghcr.io/simcesplatform/my_component:latest` as an example and assumes that the image in question has been already built and that the login to the container registry has been done.

To push the image to GitHub Container registry:

    :::bash
    docker push ghcr.io/simcesplatform/my_component:latest

Optional steps to make the new Docker image publicly available without credentials to the container registry:

- Check that the pushed image appears in the package list of the `simcesplatform` organization: [https://github.com/orgs/simcesplatform/packages](https://github.com/orgs/simcesplatform/packages)
- Go to the image page by clicking the image name (the url should be: [https://github.com/orgs/simcesplatform/packages/container/package/my_component](https://github.com/orgs/simcesplatform/packages/container/package/my_component))
- Go to the image settings page by clicking "`Package settings`" on the right-hand side of the image page (the url should be: [https://github.com/orgs/simcesplatform/packages/container/my_component/settings](https://github.com/orgs/simcesplatform/packages/container/my_component/settings))
- Change the image visibility to "`Public`" using the "`Change visibility`" option in the image settings page.
- (Optional) link the image to a repository
    - From the image page, click "`Connect repository`" button and choose the repository you want to connect the image to.
    - Afterwards, the image will be shown on the repository page as one the packages connected to the repository.
