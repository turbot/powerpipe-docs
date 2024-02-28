---
title: PIPES_INSTALL_DIR
sidebar_label: PIPES_INSTALL_DIR
---

# PIPES_INSTALL_DIR

Set the installation directory for files used with [Turbot Pipes](https://turbot.com/pipes/docs), such as login tokens. The default is `~/.pipes`.


## Usage 

Change the Pipes installation directory to `/pipes` :

```bash
export PIPES_INSTALL_DIR=/pipes
```

Reset the Pipes installation directory to the default:

```bash
unset PIPES_INSTALL_DIR
```