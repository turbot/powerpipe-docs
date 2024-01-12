---
title: FLOWPIPE_INSTALL_DIR
sidebar_label: FLOWPIPE_INSTALL_DIR
---

# FLOWPIPE_INSTALL_DIR

Set the installation directory for Powerpipe. Internal Powerpipe files will be written to this path. The default is `~/.powerpipe`.  By default, the [FLOWPIPE_CONFIG_PATH](/docs/reference/env-vars/powerpipe_config_path) is also relative to this installation directory.


## Usage 

Change the installation directory to `/powerpipe` :

```bash
export FLOWPIPE_INSTALL_DIR=/powerpipe
```

Reset the installation directory to the default:

```bash
unset FLOWPIPE_INSTALL_DIR
```