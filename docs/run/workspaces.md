---
title: Managing Workspaces
sidebar_label: Workspaces
---
# Managing Workspaces

A Flowpipe `workspace` is a "profile" that allows you to define a unified environment that the Flowpipe client can interact with.  

Flowpipe workspaces allow you to [define multiple named configurations](#defining-workspaces):

```hcl
workspace "default" {
  log_level     = "warn"
  output        = "pretty"
  memory_max_mb = 1024
}

workspace "local_server" {
  host          = "local"
  listen        = "network"
  port          = 7103
  input         = false
  memory_max_mb = 2048
}

workspace "remote_server" {
  host          = "https://flowpipe.mydomain.com:7103"
}
```

and [easily switch between them](#using-workspaces) using the `--workspace` argument or `FLOWPIPE_WORKSPACE` 
environment variable:

```bash
flowpipe server start --workspace local_server
flowpipe pipeline run my_pipeline --workspace local_server
flowpipe pipeline list --workspace remote_server
flowpipe pipeline run remote_pipeline --workspace remote_server
```

<!--
Turbot Pipes workspaces are [automatically supported](#implicit-workspaces):
```bash
flowpipe query --workspace acme/dev "select * from aws_account"
```
-->


## Defining Workspaces
[Workspace](/docs/reference/config-files/workspace) configurations can be defined in any `.fpc` file in the [configuration file search path](/docs/reference/env-vars/flowpipe_config_path) but by convention they are defined in a file named `workspaces.fpc`.  This file may contain multiple `workspace` definitions that can then be referenced by name.

Any unset arguments will assume the default values - you don't need to set them all:

```hcl
workspace "default" {
  output       = "pretty"
  update_check = true
  telemetry    = false
  port         = 7103
}
```

You can use `base=` to inherit settings from another profile:
```hcl
workspace "dev" {
  base      = workspace.default
  log_level = "warn"
}

workspace "prod" {
  base         = workspace.default
  log_level    = "info"
  input        = false
  update_check = false
  host         = local
}
```


## Using Workspaces
The workspace named `default` is special; if a workspace named `default` exists,
`--workspace` is not  specified in the command, and `FLOWPIPE_WORKSPACE` is not set, then Flowpipe uses the `default` workspace:

```bash
flowpipe pipeline run my_pipeline
```

It is common to create the `default` workspace in `~/.flowpipe/config/workspaces.fpc`.  You can also create a "directory local" default for a specific directory.  Because the default [configuration file search path](/docs/reference/env-vars/flowpipe_config_path) includes the current directory / mod location at a higher precedence than `~/.flowpipe/config`, your "directory local" default will take precedence when you run Flowpipe from that directory.

You can pass any workspace to `--workspace` to use its values:

```bash
flowpipe pipeline run my_pipeline --workspace=dev 
```

Or do the same with the `FLOWPIPE_WORKSPACE` environment variable:

```bash
FLOWPIPE_WORKSPACE=dev flowpipe pipeline run my_pipeline 
```
If you specify the `--workspace` argument and the `FLOWPIPE_WORKSPACE` environment variable, the `--workspace` argument wins:

```bash
# prod will be used as the effective workspace
export FLOWPIPE_WORKSPACE=dev 
flowpipe pipeline run my_pipeline --workspace=prod
```

If you specify the `--workspace` argument and more specific arguments, any more specific arguments will override the workspace values:

```bash
# will use "local" as the host, but dev workspace for any OTHER options
flowpipe pipeline run my_pipeline --workspace=dev --host=local
```

Environment variable values override `default` workspace settings when the `default` workspace is *implicitly used*:
```bash
# will use 7777 as the port, but get the rest of the values from the default workspace
export FLOWPIPE_PORT=7777 
flowpipe server 
```

If the default  workspace is *explicitly* passed to the `--workspace` argument, its values will override any individual environment variables:

```bash
# will NOT use 7777 as port - will use ALL of the values from default workspace so the port is 7103
export FLOWPIPE_PORT=7777 
flowpipe server --workspace=default 
```

The same is true of any named workspace:
```bash
# will NOT use 7777 as port - will use ALL of the values from prod workspace so the port is 7103
export FLOWPIPE_PORT=7777 
flowpipe server --workspace=prod 
```

<!--
## Implicit Workspaces

Named workspaces follow normal standards for HCL identifiers, thus they cannot contain
the slash (`/`) character.  If you pass a value to `--workspace` or `FLOWPIPE_WORKSPACE`
in the form of `{identity_handle}/{workspace_handle}`, it will be interpreted as
an **implicit workspace**.  Implicit workspaces, as the name suggests, do not
need to be specified in the `workspaces.fpc` file.  Instead, they will be assumed
to refer to a Flowpipe Cloud workspace, which will be used as both the database (`workspace_database`)
and snapshot location (`snapshot_location`).

Essentially, `--workspace acme/dev` is equivalent to:
```hcl
workspace "acme/dev" {
  workspace_database = "acme/dev"
  snapshot_location  = "acme/dev"
}
```
-->