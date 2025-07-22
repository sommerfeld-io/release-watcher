FROM sommerfeldio/devcontainer:0.10.4
LABEL maintainer="sebastian@sommerfeld.io"

ARG USER=vscode

USER root

RUN apt-get update \
    && apt-get install -y --no-install-recommends software-properties-common \
    && add-apt-repository -y --update ppa:ansible/ansible \
    && apt-get install -y --no-install-recommends ansible \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

USER ${USER}
