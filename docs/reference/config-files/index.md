---
title: Configuration Files
---

# Configuration Files

Configuration resources like [workspaces](/docs/reference/config-files/workspace) are defined using HCL in one or more Powerpipe config files.  


## Configuration Search Path

Powerpipe will load ALL configuration files (`*.ppc`) from every directory in the [configuration search path](/docs/reference/env-vars/powerpipe_config_path), with decreasing precedence.  By default, the configuration search path includes the current directory (or `--mod-location` if specified) followed by the `config` directory in the [POWERPIPE_INSTALL_DIR](/docs/reference/env-vars/powerpipe_install_dir): `.:$POWERPIPE_INSTALL_DIR/config`.  This allows you to manage your workspaces centrally in the `~/.powerpipe/config` directory, but override them in the mod location if desired.