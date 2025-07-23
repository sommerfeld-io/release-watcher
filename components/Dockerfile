FROM ubuntu:24.04
LABEL maintainer="sebastian@sommerfeld.io"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        ca-certificates \
        curl \
        gnupg \
        jq \
        diffutils \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install task
ARG TASK_VERSION=3.42.1
ADD https://github.com/go-task/task/releases/download/v${TASK_VERSION}/task_linux_amd64.deb /tmp/task.deb
RUN dpkg -i /tmp/task.deb \
    && rm /tmp/task.deb

# Install Github CLI
RUN mkdir -p -m 755 /etc/apt/keyrings \
    && curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | tee /etc/apt/keyrings/githubcli-archive-keyring.gpg > /dev/null \
    && chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg \
    && echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
    && apt-get update \
    && apt-get install -y --no-install-recommends gh \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

COPY taskfile.yml /app/taskfile.yml
COPY template.md /app/template.md
WORKDIR /app

ENTRYPOINT [ "task", "run" ]
