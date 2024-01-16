---
title: Run Powerpipe
sidebar_label: Run Powerpipe
---

# Run Powerpipe

Powerpipe is simple to install and manage and does not require any special expertise to get started.  It's distributed as a single binary file - just [download and run it](/downloads)!

## Mod Location
Powerpipe always runs in the context of a [mod](/docs/build/index), which is a collection of Powerpipe pipelines and triggers.  You can [import and use resources from other mods](/docs/build/mod-dependencies) so you can get started without even writing any code! You can explore the available mods on the [Powerpipe Hub](https://hub.powerpipe.io/).

Powerpipe loads the mod from the current directory by default, but you can pass the [--mod-location](/docs/reference/cli/index) flag or set the [POWERPIPE_MOD_LOCATION](/docs/reference/env-vars/powerpipe_mod_location) to set it to a different path.  The event store is also written to this mod location, in the `.powerpipe/store/` subdirectory.

## Configuration Files
Powerpipe will load [configuration files](/docs/reference/config-files/index) (`*.fpc`) according to the configuration search path.  You can change this path with the `--config-path` argument or the [POWERPIPE_CONFIG_PATH](/docs/reference/env-vars/powerpipe_config_path) environment variable, but it defaults to `.:$POWERPIPE_INSTALL_DIR/config` (`.:~/.powerpipe/config`).  This allows you to manage your [workspaces](/docs/run/workspaces) and [credentials](/docs/run/credentials) centrally in the `~/.powerpipe/config` directory, but override them in the working directory / mod location if desired.


## Operating Modes

Powerpipe can operate in 2 modes.

By default, Powerpipe runs in **Client-only Mode**.  Powerpipe loads the mod, runs the command, and exits. [Triggers](/docs/build/triggers) are not enabled in Client-Only Mode.

If you run Powerpipe in **Server Mode** mode, Powerpipe will run an API server (on port `7103` by default).  In [this mode](/docs/run/server), you can create [triggers](/docs/powerpipe-hcl/trigger/index) that run pipelines [on a schedule](/docs/powerpipe-hcl/trigger/schedule) or in response to events such as [webhooks](/docs/powerpipe-hcl/trigger/http).  After you start the Powerpipe server, you can run Powerpipe commands against it by specifying the [--host](/docs/reference/cli/index) argument.
