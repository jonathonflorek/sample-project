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

SSH into port 222 of your current machine if Linux or the IP of your WSL VM if Docker Desktop. The credentials are `root:devtools`.
The root password can be reconfigured on `docker compose up` time with the `DEVTOOLS_PASSWORD` environment variable.

For a visual experience, use the `terminator` terminal emulator.

For a modern development experience, access https://localhost:8443/

> **_NOTE - Windows users on Docker Desktop_**
> 
> For Windows users on Docker Desktop, you can use `ipconfig` to get the IP of the WSL VM for SSH, and use `xlaunch` to start an X11 server locally.
> This IP will be used in the web browser as well.

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

Kind and Docker are preloaded on this development image, and the provided `docker-compose.yml` will launch `docker:dind` container alongside the main devcontainer for 'nibling' docker-in-docker.

To start a Kubernetes cluter, navigate to the root directory of the cloned cluster.

```sh
./tools/kind-up
./tools/ingress-up
```

With this configuration, an empty ingress-nginx controller will serve at ports 80 and 443 of your local machine.

### Starting Documentation Server

Mkdocs is bundled into the development environment as well. To publish a live HTTP server on port 8000:

```sh
./tools/mkdocs-serve
```

Access the documentation at `http://localhost:8000/` and a PDF export of the documentation at `http://localhost:8000/pdf/document.pdf` (for Windows, replace `localhost` with the WSL IP)

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

# Current Security Holes

- The default root password is `devtools`. The user should be warned quite obviously when their root password is insecure. Possibly by using PS1
