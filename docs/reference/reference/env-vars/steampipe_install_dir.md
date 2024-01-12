---
title: STEAMPIPE_INSTALL_DIR
sidebar_label: STEAMPIPE_INSTALL_DIR
---

# STEAMPIPE_INSTALL_DIR
Sets the directory for the Powerpipe installation, in which the Powerpipe database, plugins, and supporting files can be found.

Powerpipe is distributed as a single binary - when you install Powerpipe, either via `brew install` or via the `curl` script, the `powerpipe` binary is installed into your path.  The first time that you run Powerpipe, it will download and install the embedded database, foreign data wrapper extension, and other required files.   By default, these files are installed to `~/.powerpipe`, however you can change this location with the `STEAMPIPE_INSTALL_DIR` environment variable or the `--install-dir` command line argument.

Powerpipe will read the `STEAMPIPE_INSTALL_DIR` variable each time it runs; if it's not set, Powerpipe will use the default path (`~/.powerpipe`). If you wish to **ALWAYS** run Powerpipe from the alternate path, you should set your environment variable in a way that will persist across sessions (in your `.profile` for example).

To install a new Powerpipe instance into an alternate path, simply specify the path in `STEAMPIPE_INSTALL_DIR` and then run a `powerpipe` command (alternatively, use the `--install-dir` argument).  If the specified directory is empty or files are missing, Powerpipe will install and update the database and files to `STEAMPIPE_INSTALL_DIR`.  If the directory does not exist, Powerpipe will create it.  

It is possible to have multiple, parallel powerpipe instances on a given machine using `STEAMPIPE_INSTALL_DIR` as long as they are running on a different port.



## Usage 
Set the STEAMPIPE_INSTALL_DIR to `~/mypath`.  You will likely want to set this in your `.profile`.
```bash
export STEAMPIPE_INSTALL_DIR=~/mypath
```
