---
title: FLOWPIPE_CONFIG_PATH
sidebar_label: FLOWPIPE_CONFIG_PATH
---

# FLOWPIPE_CONFIG_PATH

## Usage 

 Sets the search path for [configuration files](/docs/reference/config-files/index).  `FLOWPIPE_CONFIG_PATH` accepts a colon-separated list of directories.  
 
 All configuration files (`*.fpc`) will be loaded from each path, with decreasing precedence.  The default is the mod location, followed by the `config` directory in the [FLOWPIPE_INSTALL_DIR](/docs/reference/env-vars/flowpipe_install_dir): `.:$FLOWPIPE_INSTALL_DIR/config`.  This allows you to manage your [workspaces](/docs/reference/config-files/workspace) and [credentials](/docs/reference/config-files/credential/index) centrally in the `~/.flowpipe/config` directory, but override them in the working directory / mod location if desired.


## Usage

Set the configuration search path to the current mod location, followed by `/flowpipe`
```bash
export FLOWPIPE_CONFIG_PATH=.:/flowpipe
```

Set a the configuration search path to `~/.flowpipe/config` but don't include the mod location
```bash
export FLOWPIPE_CONFIG_PATH=~/.flowpipe/config
```

Reset the configuration search path to the default:
```bash
unset FLOWPIPE_CONFIG_PATH
```