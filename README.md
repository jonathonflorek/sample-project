# sample-project
A sample project

## Getting Started
The Sample Project comes with a dedicated development environment powered by Docker.

To launch the development environment, clone the repo. 

> **_NOTE - Reducing Clone Time_**
> 
> A shallow clone and sparse checkout of the `utils/` directory would be sufficient, if download time is a concern. The repo will be cloned again from within the devcontainer.

Within the `utils/` directory, launch the docker container daemon.

```sh
docker compose up --build -d
```

Exec into the `devcontainer` container with the `entrypoint` command, forwarding your X11 `DISPLAY` variable.

```sh
docker exec -it devcontainer -e DISPLAY=$DISPLAY entrypoint
```

> **_NOTE - Windows users on Docker Desktop_**
> 
> For Windows users on Docker Desktop, use `xlaunch` to start an X11 server locally **with the option `No Access Control` enabled**.
> Assemble your `DISPLAY` value from the port and display number provided by xMing on the system server tray (for example, `0.0`) and your machine's local IP address, provided with `ipconfig` in the command prompt.
> 
> For example, your `DISPLAY` value could be `169.254.32.2:0.0`.
