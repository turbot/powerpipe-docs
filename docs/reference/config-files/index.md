---
title: Configuration Files
sidebar_label: Configuration Files
---

# Configuration Files

Configuration resources like [workspaces](/docs/reference/config-files/workspace) and [credentials](/docs/reference/config-files/credential/index) are defined using HCL in one or more Powerpipe config files.  

Powerpipe will load ALL configuration files (`*.fpc`) from every directory in the [configuration search path](/docs/reference/env-vars/powerpipe_config_path), with decreasing precedence.  By default, the configuration search path includes the current directory (or `--mod-location` if specified) followed by the `config` directory in the [POWERPIPE_INSTALL_DIR](/docs/reference/env-vars/powerpipe_install_dir): `.:$POWERPIPE_INSTALL_DIR/config`.  This allows you to manage your workspaces and credentials centrally in the `~/.powerpipe/config` directory, but override them in the working directory / mod location if desired.