---
title:  workspace
sidebar_label: workspace
---
# workspace 

A Powerpipe `workspace` is a "profile" that allows you to define options for running Powerpipe.  

```hcl
workspace "default" {
  update_check  = true
  log_level     = "info"
  memory_max_mb = "2048"
  listen        = "local"

}

workspace "my_server" {
  host          = "local"
  listen        = "network"
  port          = 9194
  update_check  = false
  memory_max_mb = 2048
}
```

Powerpipe workspaces allow you to define multiple named configurations and easily switch between them using the `--workspace` argument or `POWERPIPE_WORKSPACE` 
environment variable. 

```bash
powerpipe server --workspace my_server
```

To learn more, see **[Managing Workspaces →](/docs/run/workspaces)**


## Arguments

| Argument            |    Default                   | Description &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;
|---------------------|------------------------------|-----------------------------------------
| `base`              | none                         | A reference to a named workspace resource that this workspace should source its definition from. Any argument can be overridden after sourcing via base.
| `cloud_host`        | `pipes.turbot.com`          | Set the Turbot Pipes host for connecting to Turbot Pipes workspace.
| `cloud_token`       | Token from`powerpipe login` | Set the Turbot Pipes authentication token for connecting to a Turbot Pipes workspace.  This may be a token obtained by `powerpipe login` or a user-generated [token](https://turbot.com/pipes/docs/da-settings#tokens).
| `database`          | `postgres://steampipe@` <br /> `127.0.0.1:9193/steampipe`| A database connection string or [Turbot Pipes workspace](https://pipes.turbot.com) to use as the default database.  The default is a local [Steampipe](https://steampipe.io) instance.
| `header`            | `true`                       | Enable or disable column headers.
| `host`              | none                         | Set the remote Powerpipe API host to connect to.  This allows you to run Powerpipe commands against a powerpipe host instead of the current working directory.
| `input`             | `true`                       | Enable/Disable interactive prompts for missing variables.  To disable prompts and fail on missing variables, set it to `false`. This is useful when running from scripts.
| `listen`            | `network`                    | Specifies the IP addresses on which `powerpipe server` will listen for connections from clients. Currently supported values are `local` (localhost only) or `network` (all IP addresses).
| `log_level`         | off                          | Set the logging output level
| `max_parallel`      | `10`                         | Set the maximum number of parallel executions. This is essentially a connection pool size; Powerpipe will attempt to run up to this many queries in parallel.
| `memory_max_mb`     | `1024`                       | Set a memory soft limit for the powerpipe process. Set to 0 to disable the memory limit.
| `output`            | `pretty`                     | Set the console output format: `pretty`, `plain`, `yaml` or `json`.
| `port`              | `9194`                       | Specifies the TCP port on which `powerpipe server` will listen for connections from clients. 
| `progress`          | `true`                       | Enable or disable progress information.
| `query_timeout`     | `240`                        | The maximum time (in seconds) a query is allowed to run before it times out.
| `search_path`       | `public`, then alphabetical  | A comma-separated list of connections to use as a custom search path for the control run. This setting only applies to Postgres databases (including Steampipe).
| `search_path_prefix`| none                         | A comma-separated list of connections to use as a prefix to the current search path for the control run. This setting only applies to Postgres databases (including Steampipe).
| `separator`         | `,`                          | Set csv output separator.
| `snapshot_location` | Pipes user's workspace       | Set the Turbot Pipes workspace or filesystem path for writing snapshots.
| `telemetry`         | `info`                       | Set the telemetry level in Powerpipe: `info` or `none` 
| `timing`            | `false`                      | Enable or disable query execution timing.
| `update_check`      | `true`                       | Enable or disable automatic update checking.
| `watch`             | `true`                       | Watch .mod files for changes when running `powerpipe server`.


Workspaces are defined using the `workspace` block in one or more Powerpipe config files.  Powerpipe will load ALL configuration files (`*.ppc`) from every directory in the [configuration search path](/docs/reference/env-vars/powerpipe_config_path), with decreasing precedence. The set of workspaces is the union of all workspaces defined in these directories.  

The workspace named `default` is special; If a workspace named `default` exists, it will be used whenever the `--workspace` argument is not passed to Powerpipe.  Creating a `default` workspace in `~/.powerpipe/config/workspaces.ppc` provides a way to set all defaults.


Note that the HCL argument names are the same as the equivalent CLI argument names,
except using underscore in place of dash:

| Workspace Argument | Environment Variable    | Argument             
|--------------------|-------------------------|----------------------
| `cloud_host`       | [POWERPIPE_CLOUD_HOST](/docs/reference/env-vars/powerpipe_cloud_host), [PIPES_HOST](/docs/reference/env-vars/pipes_host) | `--cloud-host`
| `cloud_token`      | [POWERPIPE_CLOUD_TOKEN](/docs/reference/env-vars/powerpipe_cloud_token), [PIPES_TOKEN](/docs/reference/env-vars/pipes_token) | `--cloud-token`
| `database`         | [POWERPIPE_DATABASE](/docs/reference/env-vars/powerpipe_database) | `--database`
| `header`           |                         | `--header`
| `host`             | [POWERPIPE_HOST](/docs/reference/env-vars/powerpipe_host)        | `--host`
| `input`            |                         | `--input` 
| `listen`           | [POWERPIPE_LISTEN](/docs/reference/env-vars/powerpipe_listen)  | `--listen` 
| `log_level`        | [POWERPIPE_LOG_LEVEL](/docs/reference/env-vars/powerpipe_log_level)   |
| `max_parallel`     | [POWERPIPE_MAX_PARALLEL](/docs/reference/env-vars/powerpipe_max_parallel) | `--max-parallel`
| `memory_max_mb`    | [POWERPIPE_MEMORY_MAX_MB](/docs/reference/env-vars/powerpipe_memory_max_mb) |
| `output`           |                         | `--output`
| `port`             | [POWERPIPE_PORT](/docs/reference/env-vars/powerpipe_port)         | `--port`
| `progress`         |                          | `--progress`
| `query_timeout`    | [POWERPIPE_QUERY_TIMEOUT](/docs/reference/env-vars/powerpipe_query_timeout)v| `--query_timeout`
| `search_path`      | none                     | `--search-path`
| `search_path_prefix` | none                   | `--search-path-prefix`
| `separator`        |                          | `--separator`
| `snapshot_location` | [POWERPIPE_SNAPSHOT_LOCATION](/docs/reference/env-vars/powerpipe_snapshot_location) | `--snapshot-location`
| `telemetry`        | [POWERPIPE_TELEMETRY](/docs/reference/env-vars/powerpipe_telemetry)   |
| `timing`           |                          | `--timing`
| `update_check`     | [POWERPIPE_UPDATE_CHECK](/docs/reference/env-vars/powerpipe_update_check) |
| `watch`            |                          | `--watch`





## Examples


```hcl

workspace "server" {
  host          = "local"
  listen        = "network"
  port          = 9194
  update_check  = false
  memory_max_mb = 2048
  log_level     = "info"
}

workspace "default" {
  output        = "pretty"
  update_check  = true
  telemetry     = "info"
  memory_max_mb = 1024
}

workspace "dev_server" {
  base          = workspace.server
  log_level     = "debug"
  port          = 9195
}

workspace "pipes" {
  cloud_host        = "vandelay.pipes.turbot.com"
  snapshot_location = "vandelay/latex"
  database          = "vandelay/latex"
}


workspace "all_options" {

  # Dashboard / API Server Options
  host                = "local"
  listen              = "network"
  port                = 9194
  watch               = true

  # General Options
  telemetry           = "info"
  update_check        = true
  log_level           = "info"
  memory_max_mb       = "1024"
  input               = true

  # Pipes Integration Options
  cloud_host          = "pipes.turbot.com"
  cloud_token         = "tpt_999faketoken99999999_111faketoken1111111111111"
  snapshot_location   = "acme/dev"

  # DB Settings
  database            = "postgres://steampipe@127.0.0.1:9193/steampipe"
  query_timeout       = 300
  max_parallel        = 5

  # Search Path Settings (Postgres-specific) 
  search_path         = "aws,aws_1,aws_2,gcp,gcp_1,gcp_2,slack,github"
  search_path_prefix  = "aws_all"

  # Output Options
  output              = "csv"  
  progress            = true
  header              = true
  separator           = ","
  timing              = true
}
```
