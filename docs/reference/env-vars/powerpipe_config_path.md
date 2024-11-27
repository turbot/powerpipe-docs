---
title: POWERPIPE_CONFIG_PATH
---

# POWERPIPE_CONFIG_PATH

Sets the search path for [configuration files](/docs/reference/config-files).  `POWERPIPE_CONFIG_PATH` accepts a colon-separated list of directories.  
 
 All configuration files (`*.ppc`) will be loaded from each path, with decreasing precedence.  The default is the mod location, followed by the `config` directory in the [POWERPIPE_INSTALL_DIR](/docs/reference/env-vars/powerpipe_install_dir): `.:$POWERPIPE_INSTALL_DIR/config`.  This allows you to manage your [workspaces](/docs/reference/config-files/workspace) and credentials centrally in the `~/.powerpipe/config` directory, but override them in the mod location if desired.


## Usage

Set the configuration search path to the current mod location, followed by `/powerpipe`
```bash
export POWERPIPE_CONFIG_PATH=.:/powerpipe
```

Set the configuration search path to `~/.powerpipe/config` but don't include the mod location
```bash
export POWERPIPE_CONFIG_PATH=~/.powerpipe/config
```

Reset the configuration search path to the default:
```bash
unset POWERPIPE_CONFIG_PATH
```
