---
title: FLOWPIPE_INSTALL_DIR
sidebar_label: FLOWPIPE_INSTALL_DIR
---

# FLOWPIPE_INSTALL_DIR

Set the installation directory for Flowpipe. Internal Flowpipe files will be written to this path. The default is `~/.flowpipe`.  By default, the [FLOWPIPE_CONFIG_PATH](/docs/reference/env-vars/flowpipe_config_path) is also relative to this installation directory.


## Usage 

Change the installation directory to `/flowpipe` :

```bash
export FLOWPIPE_INSTALL_DIR=/flowpipe
```

Reset the installation directory to the default:

```bash
unset FLOWPIPE_INSTALL_DIR
```