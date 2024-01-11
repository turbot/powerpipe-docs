---
title:  workspace
sidebar_label: workspace
---
# workspace 

A Flowpipe `workspace` is a "profile" that allows you to define options for running Flowpipe.  

```hcl

workspace "my_server" {
  host          = "local"
  listen        = "network"
  port          = 7103
  update_check  = false
  memory_max_mb = 2048
}
```

Flowpipe workspaces allow you to define multiple named configurations and easily switch between them using the `--workspace` argument or `FLOWPIPE_WORKSPACE` 
environment variable. 

```bash
flowpipe server --workspace my_server
```

To learn more, see **[Managing Workspaces →](/docs/run/workspaces)**


## Arguments

| Argument            |    Default  | Description
|---------------------|-----------------------------------------------|-----------------------------------------
| `base`              | none                         | A reference to a named workspace resource that this workspace should source its definition from. Any argument can be overridden after sourcing via base.
| `host`              | none                         | Set the remote Flowpipe API host to connect to.  This allows you to run Flowpipe commands against a flowpipe host instead of the current working directory.
| `input`             | `true`                       | Enable/Disable interactive prompts for missing variables.  To disable prompts and fail on missing variables, set it to `false`. This is useful when running from scripts.   <br /> <br /> CLI: `--input`
| `listen`            | `network`                    | Specifies the IP addresses on which `flowpipe server` will listen for connections from clients. Currently supported values are `local` (localhost only) or `network` (all IP addresses).
| `log_level`         | off                          | Set the logging output level
| `memory_max_mb`     | `1024`                       | Set a memory soft limit for the flowpipe process. Set to 0 to disable the memory limit. This can also be set via the FLOWPIPE_MEMORY_MAX_MB environment variable.
| `output`            | `pretty`                     | Set the console output format: `pretty`, `plain`, `yaml` or `json`.
| `port`              | `7103`                       | Specifies the TCP port on which `flowpipe server` will listen for connections from clients. 
| `telemetry`         | `info`                       | Set the telemetry level in Flowpipe: `info` or `none` 
| `update_check`      | `true`                       | Enable or disable automatic update checking.
| `watch`             | `true`                       | Watch .mod files for changes when running `flowpipe server`.


<!--
| `event_store`       | `$PWD/.flowpipe/flowpipe.db` | The path the the event store file. If the file does not exist, it will be created.
| `insecure`          | `false`                      | When set to `true`, ignore any TLS certificate errors and warnings when connecting to a Flowpipe API host. 

| `mod_location`      | `$PWD`                       | Set the mod working directory.



| `cache`             | `true`                                        | Enable/disable caching.  Note that is a **client**  setting -  if the database (`options "database"`) has the cache disabled, then the cache is disabled regardless of the workspace setting. <br /> <br /> Env: [STEAMPIPE_CACHE](/docs/reference/env-vars/steampipe_cache)
| `cache_ttl`         | `300`                                         | Set the client query cache expiration (TTL) in seconds.  Note that is a **client**  setting - if the database `cache_max_ttl` is lower than the `cache_ttl` in the workspace, then the effective ttl for this workspace is the `cache_max_ttl`. <br /> <br /> Env: [STEAMPIPE_CACHE_TTL](/docs/reference/env-vars/steampipe_cache_ttl)

| `cloud_host`        | `cloud.steampipe.io`                          | Set the Turbot Pipes host for connecting to Turbot Pipes workspace.
| `cloud_token`       | The token obtained by `steampipe login`       | Set the Turbot Pipes authentication token for connecting to a Turbot Pipes workspace.  This may be a token obtained by `steampipe login` or a user-generated [token](/docs/cloud/profile#tokens).



| `max_parallel` | `10` | an integer| Set the maximum number of parallel executions. When running pipelines, Flowpipe will attempt to run up to this many steps in parallel. This can also be set via the  `FLOWPIPE_MAX_PARALLEL` environment variable.

| `query_timeout`     | `240` for controls, unlimited otherwise       | The maximum time (in seconds) a query is allowed to run before it times out.



| `search_path`       | `public`, then alphabetical                   | A comma-separated list of connections to use as a custom search path for the control run. See also: [Using search_path to target connections and aggregators](https://steampipe.io/docs/guides/search-path).
| `search_path_prefix`| none                                          | A comma-separated list of connections to use as a prefix to the current search path for the control run. 

| `theme`             | `dark`                                        | Select the output theme (color scheme, etc) when running `steampipe check`.  Possible values are `light`,`dark`, and `plain`  <br /> <br />CLI: `--theme` 

| `workspace_database`| `local`                                       | Workspace database. This can be a local or remote Turbot Pipes database.
-->



Workspaces are defined using the `workspace` block in one or more Flowpipe config files.  Flowpipe will load ALL configuration files (`*.fpc`) from every directory in the [configuration search path](/docs/reference/env-vars/flowpipe_config_path), with decreasing precedence. The set of workspaces is the union of all workspaces defined in these directories.  

The workspace named `default` is special; If a workspace named `default` exists, it will be used whenever the `--workspace` argument is not passed to Flowpipe.  Creating a `default` workspace in `~/.flowpipe/config/workspaces.fpc` provides a way to set all defaults.


Note that the HCL argument names are the same as the equivalent CLI argument names,
except using underscore in place of dash:

| Workspace Argument | Environment Variable    | Argument             
|--------------------|-------------------------|----------------------
| `host`             | `FLOWPIPE_HOST`         | `--host`
| `input`            |                         | `--input` 
| `listen`           | `FLOWPIPE_LISTEN`       | `--listen` 
| `log_level`        | `FLOWPIPE_LOG_LEVEL`    |
| `memory_max_mb`    | `FLOWPIPE_MEMORY_MAX_MB`|
| `output`           |                         | `--output`
| `port`             | `FLOWPIPE_PORT`         | `--port`
| `telemetry`        | `FLOWPIPE_TELEMETRY`    |
| `update_check`     | `FLOWPIPE_UPDATE_CHECK` | 
| `watch`            |                         | `--watch`



<!--
| `insecure`         | `FLOWPIPE_INSECURE`     | `--insecure` 
| `event_store`      |                         | `--event-store`

| `mod_location`     | `FLOWPIPE_MOD_LOCATION` | `--mod-location`


| `cloud_host`                  | `FLOWPIPE_CLOUD_HOST`         | `--cloud-host`       |
| `cloud_token`                 | `FLOWPIPE_CLOUD_TOKEN`        | `--cloud-token`      |

| `query_timeout`               | `FLOWPIPE_QUERY_TIMEOUT`      | `--query_timeout`     |
| `workspace_database`          | `FLOWPIPE_WORKSPACE_DATABASE` | `--workspace-database`|


| `search_path`                 | none                           | `--search-path`       |
| `search_path_prefix`          | none                           | `--search-path-prefix`|

| `max_parallel`                | `FLOWPIPE_MAX_PARALLEL`       | `--max-parallel`      |


-->


## Examples


```hcl

workspace "server" {
  host          = "local"
  listen        = "network"
  port          = 7103
  update_check  = false
  memory_max_mb = 2048
}

workspace "aws_01" {
  output        = "pretty"
  watch         = true
  input         = true
  host          = "http://localhost:7103"
  port          = 7103
  listen        = local
  update_check  = true
  telemetry     = "info"
  log_level     = "info"
  memory_max_mb = 1024

}

workspace "dev_server" {
  base          = workspace.server
  log_level     = "debug"
  port          = 7104
}
```