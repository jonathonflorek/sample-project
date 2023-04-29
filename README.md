# sample-project
A sample project

## Getting Started
The Sample Project comes with a dedicated development environment powered by Docker.

### Requirements

The following applications are required to host the development environment:
- Docker with compose extension (>=20.10.10; see https://github.com/moby/moby/issues/42680)
- Git, or a curl-equivalent

### Instructions

To launch the development environment, download the repo and launch the docker daemon.

```sh
git clone https://github.com/jonathonflorek/sample-project.git
cd sample-project/devtools
docker compose up --build -d
```

Navigate to `https://localhost:8443` (for Windows, replace `localhost` with the WSL IP). 
The password is `devtools` and can be changed by setting `DEV_PASSWORD` environment variable when running `docker compose up`.

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
./devtools/scripts/kind-up
./devtools/scripts/ingress-up
```

Access the empty ingress-nginx controller at `http://localhost` and `https://localhost` (for Windows, replace `localhost` with the WSL IP),

### Starting Documentation Server

Mkdocs is bundled into the development environment. To publish a live HTTP server on port 8080:

```sh
./devtools/scripts/mkdocs-serve
```

Access the documentation at `http://localhost:8080/` and a PDF export of the documentation at `http://localhost:8080/pdf/document.pdf` (for Windows, replace `localhost` with the WSL IP).

# Current Security Holes

- The default root password is `devtools`. The user should be warned quite obviously when their root password is insecure. Possibly by using PS1
