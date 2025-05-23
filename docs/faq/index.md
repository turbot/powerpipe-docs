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

Powerpipe has a dependency on the GNU C Library (glibc) and requires a minimum glibc version of 2.29. As a result, Powerpipe is supported and rigorously tested on the latest Linux Long-Term Support (LTS) distributions to ensure reliability and stability.

#### Supported Distributions and Versions

| Distribution       | Version | glibc Version | Notes                                                   |
|--------------------|---------|---------------|---------------------------------------------------------|
| Ubuntu LTS         | 24.04   | 2.39          |                                                         |
| Ubuntu             | 22      | 2.35          | To cover Windows WSL2, which may be behind              |
| CentOS (Stream)    | 9       | 2.34          |                                                         |
| RHEL               | 9       | 2.34          |                                                         |
| Amazon Linux       | 2023    | 2.34          |                                                         |

#### Policy Overview

- **Minimum glibc Requirement**: Powerpipe requires glibc version 2.29 or higher. It will not function on systems with an older glibc version.
- **Tested Distributions**: Powerpipe is tested on the latest versions of Linux LTS distributions. While it may work on other distributions with the required glibc version, official support and testing are limited to those listed above.

This policy ensures that Powerpipe operates smoothly on supported environments, maintaining the highest standards of reliability and performance.