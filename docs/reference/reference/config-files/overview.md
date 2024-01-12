---
title: Configuration Files
sidebar_label: Configuration Files
---

# Configuration Files

Configuration file resource are defined using HCL in one or more Powerpipe config files.  Powerpipe will load ALL configuration files from `~/.powerpipe/config` that have a `.spc` extension.  


Typically, config files are laid out as follows:
- Powerpipe creates a `~/.powerpipe/config/default.spc` file for setting [options](reference/config-files/options).
- Each plugin creates a `~/.powerpipe/config/{plugin name}.spc` (e.g. `aws.spc`, `github.spc`, `net.spc`, etc). Define your [connections](reference/config-files/connection) and [plugins](reference/config-files/plugin) in these files.
- Define your [workspaces](reference/config-files/workspace) in `~/.powerpipe/config/workspaces.spc`.