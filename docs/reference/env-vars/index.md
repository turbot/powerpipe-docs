---
title:  Environment Variables
sidebar_label:  Environment Variables
---

# Environment Variables

Flowpipe supports environment variables to allow you to change its default behavior.  These are optional settings - You are not required to set any environment variables.

Note that plugins may also support environment variables, but these are plugin-specific - refer to your plugin's documentation on the [Flowpipe Hub](https://hub.flowpipe.io/) for details.

## Flowpipe Environment Variables

| Command | Default | Description
|-|-|-
| [FLOWPIPE_CONFIG_PATH](reference/env-vars/flowpipe_config_path)  | `.:$FLOWPIPE_INSTALL_DIR/config` | Sets the search path for [configuration files](/docs/reference/config-files/index).  `FLOWPIPE_CONFIG_PATH` accepts a colon-separated list of directories.  
| [FLOWPIPE_HOST](reference/env-vars/flowpipe_host)  | none | Set the remote Flowpipe API host to connect to.  This allows you to run Flowpipe commands against a flowpipe host instead of the current working directory / mod location.
| [FLOWPIPE_INSTALL_DIR](reference/env-vars/flowpipe_install_dir)  | `~/.flowpipe` | Set the installation directory for flowpipe. Internal flowpipe files will be written to this path.
| [FLOWPIPE_LISTEN](reference/env-vars/flowpipe_listen)  | `network` | Specifies the IP addresses on which `flowpipe server` will listen for connections from clients. Currently supported values are `local` (localhost only) or `network` (all IP addresses).
| [FLOWPIPE_LOG_LEVEL](reference/env-vars/flowpipe_log_level)  | off | Set the logging output level
| [FLOWPIPE_MEMORY_MAX_MB](reference/env-vars/flowpipe_memory_max_mb)  | `1024` | Set a soft memory limit for the `flowpipe` process. 
| [FLOWPIPE_MOD_LOCATION](reference/env-vars/flowpipe_mod_location)  | current working directory | Set the workspace working directory
| [FLOWPIPE_PORT](reference/env-vars/flowpipe_port)  | `7103` | Specifies the TCP port on which `flowpipe server` will listen for connections from clients. 
| [FLOWPIPE_TELEMETRY](reference/env-vars/flowpipe_telemetry)  | `info` | Set the level of telemetry data to collect and send
| [FLOWPIPE_UPDATE_CHECK](reference/env-vars/flowpipe_update_check)| `true` | Enable/disable automatic update checking
| [FLOWPIPE_WORKSPACE](reference/env-vars/flowpipe_workspace)  | `default` | Set the Flowpipe workspace .  This can be named workspace from `workspaces.fpc` or a remote Flowpipe Cloud workspace




<!--

| [FLOWPIPE_INSECURE](reference/env-vars/flowpipe_insecure)  | `false` | When set to `true`, ignore any TLS certificate errors and warnings when connecting to a Flowpipe API host.


| [FLOWPIPE_CACHE](reference/env-vars/flowpipe_cache)| `true` | Enable/disable caching [DEPRECATED]
| [FLOWPIPE_CACHE_TTL](reference/env-vars/flowpipe_cache_ttl)| `300` | The amount of time to cache results, in seconds [DEPRECATED]

| [FLOWPIPE_MAX_PARALLEL](reference/env-vars/flowpipe_max_parallel)  | `10` | Set the maximum number of parallel executions

| [FLOWPIPE_QUERY_TIMEOUT](reference/env-vars/flowpipe_query_timeout)  |  `240` for controls, unlimited in all other cases. | Set the amount of time to wait for a query to complete before timing out, in seconds.

| [FLOWPIPE_CLOUD_HOST](reference/env-vars/flowpipe_cloud_host)  | `cloud.flowpipe.io` | Set the Flowpipe Cloud host, for connecting to Flowpipe Cloud workspace
| [FLOWPIPE_CLOUD_TOKEN](reference/env-vars/flowpipe_cloud_token)  |  | Set the Flowpipe Cloud authentication token for connecting to Flowpipe Cloud workspace



| [FLOWPIPE_WORKSPACE_DATABASE](reference/env-vars/flowpipe_workspace_database)  | `local` | Workspace database.  This can be `local` or a remote Flowpipe Cloud database


-->