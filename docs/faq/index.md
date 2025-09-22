---
title:  FAQ
---

# FAQ

## Where is the Dockerfile or container example?

Powerpipe can be run in a containerized setup. We run it ourselves that way as part of [Turbot Pipes](https://turbot.com/pipes). However, we don't publish or support a container definition because:

* The CLI is optimized for developer use on the command line.
* Everyone has specific goals and requirements for their containers.
* Container setup requires various mounts and access to configuration files.
* It's hard to support containers across many different environments.

We welcome users to create and share open-source container definitions for Powerpipe.

## What Linux distributions and versions are officially supported by Powerpipe?

Powerpipe requires glibc version 2.29 or higher and libstdc++ (GCC runtime) with symbol version GLIBCXX_3.4.30 or higher. It will not function on systems with an older glibc or an older libstdc++ runtime.

Powerpipe is tested on the latest versions of Linux LTS distributions. While it may work on other distributions with the required glibc and libstdc++ versions, official support and testing are limited to the following:


| Distribution       | Version | glibc Version | Notes                                                   |
|--------------------|---------|---------------|---------------------------------------------------------|
| Ubuntu LTS         | 24.04   | 2.39          |                                                         |
| Ubuntu             | 22      | 2.35          | To cover Windows WSL2, which may be behind              |
| CentOS (Stream)    | 10      | 2.39          |                                                         |
| RHEL               | 10      | 2.39          |                                                         |
| Amazon Linux       | 2023    | 2.34          |                                                         |