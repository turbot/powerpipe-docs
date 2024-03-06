---
title: Container Usage Guide
sidebar_label: Container Usage Guide
---

# Container Usage Guide

Turbot's Powerpipe is available as a Docker container image, pre-configured for seamless integration with Docker environments. This guide provides instructions on how to deploy and run Powerpipe using Docker, including configurations for different operating systems and ensuring proper user permissions.

## Getting Started

### Pulling the Powerpipe Image

First, obtain the Powerpipe container image from the GitHub Container Registry by executing the following command:

```bash
docker pull ghcr.io/turbot/powerpipe:latest
```

### Running Powerpipe Locally

To start using Powerpipe immediately, you can run the container with a simple command:

```bash
docker run \
    -it \
    --rm \
    --name powerpipe \
    ghcr.io/turbot/powerpipe:latest
```

## Configurations for Enhanced Security and Functionality

### Setting up Network Connectivity

The Powerpipe container doesn't come with a preinstalled database and requires one for data sourcing.
To establish connectivity, configuring network settings is necessary.
This involves connecting the container to a network with a running database.

In the following examples, we'll demonstrate how to connect the Powerpipe container to either the host machine network or to another container running a data source by creating a Docker network for communication.

To connect Powerpipe to the host network, execute the following command:

```bash
docker run \
    -it \
    --rm \
    --name powerpipe \
    --network host \
    ghcr.io/turbot/powerpipe:latest
```

Alternatively, if you're running a data source in a container, you can establish a Docker network and connect Powerpipe to it. 
Ensure that the container running the database is also connected to this network. 

Follow these steps:

```bash
# Create a Docker network
docker network create powerpipe_network

# Connect Powerpipe container to the created network
docker run \
    -it \
    --rm \
    --name powerpipe \
    --network powerpipe_network \
    ghcr.io/turbot/powerpipe:latest
```

By following these instructions, you can integrate Powerpipe with your database setup for data retrieval and processing.

### Setting Environment Variables for Linux User Permissions

Deploying Powerpipe on a Linux system may require setting specific environment variables to manage file ownership. Adding `-e USER_UID=$(id -u)` and `-e USER_GID=$(id -g)` to your Docker run command aligns the container's file and process ownership with that of the host system. This adjustment ensures accurate file ownership in mounted volumes.

Example command incorporating these environment variables:

```bash
docker run \
    -it \
    --rm \
    --name powerpipe \
    -e USER_UID=$(id -u) \
    -e USER_GID=$(id -g) \
    ghcr.io/turbot/powerpipe:latest
```

## Running Locally as an Alternative to Standard Installation

To simplify the usage of Powerpipe as if it were locally installed, you can create a Docker alias.
This alias enables running the Powerpipe container as a local command.
It binds your active directories to the '/workspace' directory within the running container.

### Preparation

Before proceeding, create example directories for Powerpipe's configuration and workspace using the following commands:

```bash
mkdir -p $HOME/pp/config
mkdir -p $HOME/pp/workspace
```

For simplicity, we will use Steampipe as the data source. If you haven't already, [install Steampipe](https://steampipe.io/downloads).
By default, Steampipe will use the default AWS credentials from your credential file and/or environment variables.
Once Steampipe is installed, install the [AWS plugin for Steampipe](https://hub.steampipe.io/plugins/turbot/aws).

```bash
steampipe plugin install aws
```

Start the [Steampipe service](https://steampipe.io/docs/managing/service). The Steampipe database needs to be running for Powerpipe to connect to it.

```bash
steampipe service start
```

Next, set up aliases for your operating system.

### macOS Setup

For macOS users, setting up the alias is straightforward.
It involves mounting the drives we created earlier and sharing the host's network stack.
To set up the alias, execute the following command:

```bash
alias pp="docker run \
    -it \
    --rm \
    --name powerpipe \
    --network host \
    --mount type=bind,source=$HOME/pp/config,target=/home/powerpipe/.powerpipe/config \
    --mount type=bind,source=$(pwd),target=/workspace \
    ghcr.io/turbot/powerpipe"
```

### Linux Setup

Linux users require a slightly different setup to ensure smooth operation of the mounted volumes by passing local user information to the container and sharing the host's network stack.

To configure the alias for Linux users, execute the following command:

```bash
alias pp="docker run \
  -it \
  --rm \
  --name powerpipe \
  --mount type=bind,source=$HOME/pp/config,target=/home/powerpipe/.powerpipe/config \
  --mount type=bind,source=$(pwd),target=/workspace \
  --network host \
  -e USER_UID=$(id -u) \
  -e USER_GID=$(id -g) \
  ghcr.io/turbot/powerpipe"
```

By utilizing this alias, Linux users ensure that Powerpipe operates with the necessary permissions and configurations, allowing for smooth interaction with mounted volumes and ensuring effective write capabilities within the container environment.

### Using Powerpipe with Docker

#### Running a query

Once the alias is set up, you can start using Powerpipe commands as though the application were installed locally on your system.
To confirm Powerpipe is running, run the following command.

```bash
pp query run "select title from aws_account"
```

#### Running a Query

Once the alias is set up, you can start using Powerpipe commands as though the application were installed locally on your system.
To confirm Powerpipe is running, execute the following command:

```bash
pp query run "select title from aws_account"
```

#### Running Dashboards

Powerpipe runs in the context of a [mod](/docs/build/). Powerpipe loads the mod from the [mod location](/docs/run#mod-location), which defaults to the `/workspace` directory on the container.

Initialize the Powerpipe mod:

```bash
pp mod init
```

Install the AWS Insights mod:

```bash
pp mod install github.com/turbot/steampipe-mod-aws-insights
```

Then, start the Powerpipe server on the container:

```bash
pp server
```

Once the server is running, use a web browser to navigate to `http://localhost:9033` to view the dashboards.

## Running in read-only mode

It is possible to run the powerpipe container with a read-only root filesystem, but note the following:

- `/tmp` must be writable (mount with tmpfs)
- config (`/home/powerpipe/.powerpipe/config`) must be writable
- workspace working directory must be writable
- you cannot use read-only containers if you passing across the environment variables `USER_UID` or `USER_GID`

To run the container as a read only root system by passing the docker flag `--read-only`, you can create the alias for `fp` as follows:

On macOS use:

```bash
alias fp="docker run \
    -it \
    --rm \
    --read-only \
    --name powerpipe \
    --mount type=bind,source=$HOME/fp/config,target=/home/powerpipe/.powerpipe/config \
    --mount type=bind,source=$(pwd),target=/workspace \
    -v /var/run/docker.sock.raw:/var/run/docker.sock \
    ghcr.io/turbot/powerpipe"
```

On Linux use:

```bash
alias fp="docker run \
    -it \
    --rm \
    --read-only \
    --name powerpipe \
    --mount type=bind,source=$HOME/fp/config,target=/home/powerpipe/.powerpipe/config \
    --mount type=bind,source=$(pwd),target=/workspace \
    -v /var/run/docker.sock:/var/run/docker.sock \
    ghcr.io/turbot/powerpipe"
```

Now you can call all available command for Powerpipe as previous examples.
