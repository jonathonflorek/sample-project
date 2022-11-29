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
> A shallow clone and sparse checkout of the `utils/` directory would be sufficient, if download time is a concern. The repo will be cloned again from within the devcontainer.
> 
> Alternatively, you can download the repo's contents as a zip or tarball and extract it

Within the `utils/` directory, launch the docker container daemon.

```sh
cd sample-project/utils
```

```sh
docker compose up --build -d
```

Exec into the started container with the `entrypoint` command, forwarding your X11 `DISPLAY` variable.

```sh
docker exec -it -e DISPLAY=$DISPLAY $CONTAINER entrypoint
```

> **_NOTE - Windows users on Docker Desktop_**
> 
> For Windows users on Docker Desktop, use `xlaunch` to start an X11 server locally **with the option `No Access Control` enabled**.
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

### Developing the Development Image

To add or update the development image, we recommend you access the `utils/` directory through the symlink `utils-dev/` when running `docker compose` commands. This ensures that containers launched from within the development environment do not get prefixed with the same `utils-` prefix as the development environment itself was launched with, and enables both environments to run at the same time.


