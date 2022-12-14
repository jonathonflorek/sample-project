# sample-project
A sample project

## Getting Started
The Sample Project comes with a dedicated development environment powered by Docker.

### Requirements

The following applications are required to host the development environment:
- Docker with compose extension (>=20.10.10; see https://github.com/moby/moby/issues/42680)
- Git, or a curl-equivalent

### Instructions

To launch the development environment, clone or download the repo. 

```sh
git clone https://github.com/jonathonflorek/sample-project.git
```

> **_NOTE - Reducing Clone Time_**
> 
> A shallow clone and sparse checkout of the `devtools/` directory would be sufficient, if download time is a concern. The repo will be cloned again from within the devcontainer.
> 
> Alternatively, you can download the repo's contents as a zip or tarball and extract it

Within the `devtools/` directory, launch the docker container daemon.

```sh
cd sample-project/utils
```

```sh
docker compose up --build -d
```

For a purely terminal experience, exec into the started container with `/bin/bash`. For a more visual experience, set the `DISPLAY` variable with `-e` and try `terminator`.

```sh
docker exec -it -e DISPLAY=$DISPLAY $CONTAINER entrypoint
```

> **_NOTE - Windows users on Docker Desktop_**
> 
> For Windows users on Docker Desktop, use `xlaunch` to start an X11 server locally. If the server is running but X11 is not working, try launching X11 **with the option `No Access Control` enabled**.
> Assemble your `DISPLAY` value from the port and display number provided by xMing on the system server tray (for example, `0.0`) and your machine's local IP address, provided with `ipconfig` in the command prompt.
> 
> For example, your `DISPLAY` value could be `169.254.32.2:0.0`.

In the container's home directory, clone the repo again

```sh
cd ~
mkdir repos
cd repos
git clone https://github.com/jonathonflorek/sample-project.git
cd sample-project
```

## Development Tools

### Starting Kubernetes

Kind and Docker are preloaded on this development image, and the provided `docker-compose.yml` will launch `docker:dind` container alongside the main devcontainer for 'nibling' docker-in-docker. You will have to modify the kind config yaml to include the docker address as a SAN.

## Project Objectives

The goal of this project is to demo an enterprise-grade sample project with the following developer experience:

- developers run the same development environment, anywhere, everywhere
- developers push to the main branch
- the build pipeline performs the following:
  - pulls the desired version of the repo
  - builds the runnable applications, with online compile-time dependencies if needed
  - builds an installer image with some 'dummy' documentation
  - collects unit test results
  - runs deployment tests against the installer on online and faux-offline environments, mocked in dind
  - runs migration tests against the installer and an environment configured by a previous version of the installer which was pulled from an archive 
  - runs system tests against a deployed system
  - runs publish tests that the installer can publish to public registries of rpm, docker, etc
  - collects needed screenshots for end-user documentation from a deployed system
- the produced installer can be delivered to any end-user system (offline included)
