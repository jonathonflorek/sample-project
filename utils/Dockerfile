FROM fedora:latest
COPY ./ /usr/local/src/devcontainer
WORKDIR /usr/local/src/devcontainer
RUN /usr/local/src/devcontainer/install
WORKDIR /
ENTRYPOINT ["/usr/local/bin/entrypoint"]

