---
title:  Environment Variables
sidebar_label:  Environment Variables
---

# Environment Variables

Powerpipe supports environment variables to allow you to change its default behavior.  These are optional settings - You are not required to set any environment variables.

Note that plugins may also support environment variables, but these are plugin-specific - refer to your plugin's documentation on the [Powerpipe Hub](https://hub.powerpipe.io/) for details.

## Powerpipe Environment Variables

| Command | Default | Description
|-|-|-
| [POWERPIPE_CONFIG_PATH](reference/env-vars/powerpipe_config_path)  | `.:$POWERPIPE_INSTALL_DIR/config` | Sets the search path for [configuration files](/docs/reference/config-files/index).  `POWERPIPE_CONFIG_PATH` accepts a colon-separated list of directories.  
| [POWERPIPE_HOST](reference/env-vars/powerpipe_host)  | none | Set the remote Powerpipe API host to connect to.  This allows you to run Powerpipe commands against a powerpipe host instead of the current working directory / mod location.
| [POWERPIPE_INSTALL_DIR](reference/env-vars/powerpipe_install_dir)  | `~/.powerpipe` | Set the installation directory for powerpipe. Internal powerpipe files will be written to this path.
| [POWERPIPE_LISTEN](reference/env-vars/powerpipe_listen)  | `network` | Specifies the IP addresses on which `powerpipe server` will listen for connections from clients. Currently supported values are `local` (localhost only) or `network` (all IP addresses).
| [POWERPIPE_LOG_LEVEL](reference/env-vars/powerpipe_log_level)  | off | Set the logging output level
| [POWERPIPE_MEMORY_MAX_MB](reference/env-vars/powerpipe_memory_max_mb)  | `1024` | Set a soft memory limit for the `powerpipe` process. 
| [POWERPIPE_MOD_LOCATION](reference/env-vars/powerpipe_mod_location)  | current working directory | Set the workspace working directory
| [POWERPIPE_PORT](reference/env-vars/powerpipe_port)  | `7103` | Specifies the TCP port on which `powerpipe server` will listen for connections from clients. 
| [POWERPIPE_TELEMETRY](reference/env-vars/powerpipe_telemetry)  | `info` | Set the level of telemetry data to collect and send
| [POWERPIPE_UPDATE_CHECK](reference/env-vars/powerpipe_update_check)| `true` | Enable/disable automatic update checking
| [POWERPIPE_WORKSPACE](reference/env-vars/powerpipe_workspace)  | `default` | Set the Powerpipe workspace .  This can be named workspace from `workspaces.fpc` or a remote Powerpipe Cloud workspace




<!--

| [POWERPIPE_INSECURE](reference/env-vars/powerpipe_insecure)  | `false` | When set to `true`, ignore any TLS certificate errors and warnings when connecting to a Powerpipe API host.


| [POWERPIPE_CACHE](reference/env-vars/powerpipe_cache)| `true` | Enable/disable caching [DEPRECATED]
| [POWERPIPE_CACHE_TTL](reference/env-vars/powerpipe_cache_ttl)| `300` | The amount of time to cache results, in seconds [DEPRECATED]

| [POWERPIPE_MAX_PARALLEL](reference/env-vars/powerpipe_max_parallel)  | `10` | Set the maximum number of parallel executions

| [POWERPIPE_QUERY_TIMEOUT](reference/env-vars/powerpipe_query_timeout)  |  `240` for controls, unlimited in all other cases. | Set the amount of time to wait for a query to complete before timing out, in seconds.

| [POWERPIPE_CLOUD_HOST](reference/env-vars/powerpipe_cloud_host)  | `cloud.powerpipe.io` | Set the Powerpipe Cloud host, for connecting to Powerpipe Cloud workspace
| [POWERPIPE_CLOUD_TOKEN](reference/env-vars/powerpipe_cloud_token)  |  | Set the Powerpipe Cloud authentication token for connecting to Powerpipe Cloud workspace



| [POWERPIPE_WORKSPACE_DATABASE](reference/env-vars/powerpipe_workspace_database)  | `local` | Workspace database.  This can be `local` or a remote Powerpipe Cloud database


-->