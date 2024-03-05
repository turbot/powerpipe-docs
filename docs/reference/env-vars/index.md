---
title:  Environment Variables
sidebar_label:  Environment Variables
---

# Environment Variables

Powerpipe supports environment variables to allow you to change its default behavior.  These are optional settings - You are not required to set any environment variables.

## Powerpipe Environment Variables

| Command | Default | Description
|-|-|-
| [PIPES_HOST](reference/env-vars/pipes_host)  | `pipes.turbot.com` | Set the Turbot Pipes host, for connecting to Turbot Pipes workspace.
| [PIPES_INSTALL_DIR](reference/env-vars/pipes_install_dir)  | `~/.pipes` | Set the installation directory for files used with [Turbot Pipes](https://turbot.com/pipes/docs), such as login tokens.
| [PIPES_TOKEN](reference/env-vars/pipes_token)  |  | Set the Turbot Pipes authentication token for connecting to Turbot Pipes workspace.
| [POWERPIPE_CONFIG_PATH](reference/env-vars/powerpipe_config_path)  | `.:$POWERPIPE_INSTALL_DIR/config` | Sets the search path for [configuration files](/docs/reference/config-files).  `POWERPIPE_CONFIG_PATH` accepts a colon-separated list of directories.  
| [POWERPIPE_DATABASE](reference/env-vars/powerpipe_database)  | `postgres://steampipe@` <br /> `127.0.0.1:9193/steampipe` | A database connection string or [Turbot Pipes workspace](https://pipes.turbot.com) to use as the default database.  The default is a local [Steampipe](https://steampipe.io) instance.
| [POWERPIPE_INSTALL_DIR](reference/env-vars/powerpipe_install_dir)  | `~/.powerpipe` | Set the installation directory for powerpipe. Internal powerpipe files will be written to this path.
| [POWERPIPE_LISTEN](reference/env-vars/powerpipe_listen)  | `network` | Specifies the IP addresses on which `powerpipe server` will listen for connections from clients. Currently supported values are `local` (localhost only) or `network` (all IP addresses).
| [POWERPIPE_LOG_LEVEL](reference/env-vars/powerpipe_log_level)  | off | Set the logging output level.
| [POWERPIPE_MAX_PARALLEL](reference/env-vars/powerpipe_max_parallel)  | `10` | Set the maximum number of parallel executions.
| [POWERPIPE_MEMORY_MAX_MB](reference/env-vars/powerpipe_memory_max_mb)  | `1024` | Set a soft memory limit for the `powerpipe` process. 
| [POWERPIPE_MOD_LOCATION](reference/env-vars/powerpipe_mod_location)  | current working directory | Set the workspace working directory.
| [POWERPIPE_PORT](reference/env-vars/powerpipe_port)  | `9033` | Specifies the TCP port on which `powerpipe server` will listen for connections from clients. 
| [POWERPIPE_QUERY_TIMEOUT](reference/env-vars/powerpipe_query_timeout)  |  `300` | Set the amount of time to wait for a query to complete before timing out, in seconds.
| [POWERPIPE_SNAPSHOT_LOCATION](/docs/reference/env-vars/powerpipe_snapshot_location) | The Turbot Pipes user's personal workspace | Set the Turbot Pipes workspace or filesystem path for writing snapshots.
| [POWERPIPE_TELEMETRY](reference/env-vars/powerpipe_telemetry)  | `info` | Set the level of telemetry data to collect and send.
| [POWERPIPE_UPDATE_CHECK](reference/env-vars/powerpipe_update_check)| `true` | Enable/disable automatic update checking.
| [POWERPIPE_WORKSPACE](reference/env-vars/powerpipe_workspace)  | `default` | Set the Powerpipe workspace .  This can be named workspace from `workspaces.ppc` or a remote Powerpipe Cloud workspace.
