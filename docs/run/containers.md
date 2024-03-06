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

### Setting Environment Variables for Linux User Permissions

Deploying Flowpipe on a Linux system may require setting specific environment variables to manage file ownership. Adding `-e USER_UID=$(id -u)` and `-e USER_GID=$(id -g)` to your Docker run command aligns the container's file and process ownership with that of the host system. This adjustment ensures accurate file ownership in mounted volumes.

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
