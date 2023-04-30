FROM fedora:latest
WORKDIR /
COPY install /tmp/install
RUN /tmp/install
COPY bin/. /usr/local/bin/
COPY etc/. /etc/
COPY entrypoint /entrypoint
ENTRYPOINT /entrypoint
