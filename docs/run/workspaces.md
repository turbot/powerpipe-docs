---
title: Managing Workspaces
---

# Managing Workspaces

A Powerpipe `workspace` is a "profile" that allows you to define a unified environment that the Powerpipe client can interact with.  

Powerpipe workspaces allow you to [define multiple named configurations](#defining-workspaces):

```hcl
workspace "default" {
  log_level     = "warn"
  output        = "pretty"
  memory_max_mb = 1024
}

workspace "local_server" {
  listen        = "network"
  port          = 9033
  input         = false
  memory_max_mb = 2048
}

```

and [easily switch between them](#using-workspaces) using the `--workspace` argument 

```bash
powerpipe server start --workspace local_server
powerpipe benchmark run my_benchmark --workspace local_server

powerpipe benchmark list --workspace remote_server
powerpipe benchmark run remote_benchmark --workspace remote_server
```

or `POWERPIPE_WORKSPACE` environment variable:
```bash
export POWERPIPE_WORKSPACE=local_server
powerpipe server start
powerpipe benchmark run my_benchmark

POWERPIPE_WORKSPACE=remote_server powerpipe benchmark list
POWERPIPE_WORKSPACE=remote_server powerpipe benchmark run remote_benchmark
```


Turbot Pipes workspaces are [automatically supported](#implicit-workspaces):
```bash
powerpipe query --workspace acme/dev "select * from aws_account"
```

<!--
Turbot Pipes workspaces are [automatically supported](#implicit-workspaces):
```bash
powerpipe query --workspace acme/dev "select * from aws_account"
```
-->

## Defining Workspaces
[Workspace](/docs/reference/config-files/workspace) configurations can be defined in any `.ppc` file in the [configuration file search path](/docs/reference/env-vars/powerpipe_config_path) but by convention, they are defined in a file named `workspaces.ppc`.  This file may contain multiple `workspace` definitions that can then be referenced by name.

Any unset arguments will assume the default values - you don't need to set them all:

```hcl
workspace "default" {
  output       = "pretty"
  update_check = true
  telemetry    = false
  port         = 9033
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
}
```


The `snapshot_location` can be a Turbot Pipes workspace, in the form 
of `{identity_handle}/{workspace_handle}`: 
```hcl
workspace "acme_prod" {
  snapshot_location = "acme/prod"
}
```

If it doesn't match the `{identity_handle}/{workspace_handle}` pattern it will be interpreted to be a path to a directory in the local filesystem where snapshots should be written to:

```hcl
workspace "local" {
  snapshot_location  = "home/raj/my-snapshots" 
}
```


## Using Workspaces
The workspace named `default` is special; if a workspace named `default` exists,
`--workspace` is not  specified in the command, and `POWERPIPE_WORKSPACE` is not set, then Powerpipe uses the `default` workspace:

```bash
powerpipe benchmark run my_benchmark
```

It is common to create the `default` workspace in `~/.powerpipe/config/workspaces.ppc`.  You can also create a "directory local" default for a specific directory.  Because the default [configuration file search path](/docs/reference/env-vars/powerpipe_config_path) includes the current directory / mod location at a higher precedence than `~/.powerpipe/config`, your "directory local" default will take precedence when you run Powerpipe from that directory.

You can pass any workspace to `--workspace` to use its values:

```bash
powerpipe benchmark run my_benchmark --workspace=dev 
```

Or do the same with the `POWERPIPE_WORKSPACE` environment variable:

```bash
POWERPIPE_WORKSPACE=dev powerpipe benchmark run my_benchmark 
```

If you specify the `--workspace` argument and the `POWERPIPE_WORKSPACE` environment variable, the `--workspace` argument wins:

```bash
# prod will be used as the effective workspace
export POWERPIPE_WORKSPACE=dev 
powerpipe benchmark run my_benchmark --workspace=prod
```

If you specify the `--workspace` argument and more specific arguments, any more specific arguments will override the workspace values:

```bash
# will use "debug" as the log level, but dev workspace for any OTHER options
powerpipe benchmark run my_benchmark --workspace=dev --log-level=debug
```

Environment variable values override `default` workspace settings when the `default` workspace is *implicitly used*:
```bash
# will use 7777 as the port, but get the rest of the values from the default workspace
export POWERPIPE_PORT=7777 
powerpipe server 
```

If the default  workspace is *explicitly* passed to the `--workspace` argument, its values will override any individual environment variables:

```bash
# will NOT use 7777 as port - will use ALL of the values from default workspace so the port is 9033
export POWERPIPE_PORT=7777 
powerpipe server --workspace=default 
```

The same is true of any named workspace:
```bash
# will NOT use 7777 as port - will use ALL of the values from prod workspace so the port is 9033
export POWERPIPE_PORT=7777 
powerpipe server --workspace=prod 
```


## Implicit Workspaces

Named workspaces follow normal standards for HCL identifiers, thus they cannot contain
the slash (`/`) character.  If you pass a value to `--workspace` or `POWERPIPE_WORKSPACE`
in the form of `{identity_handle}/{workspace_handle}`, it will be interpreted as
an **implicit workspace**.  Implicit workspaces, as the name suggests, do not
need to be specified in the `workspaces.ppc` file.  Instead, they will be assumed
to refer to a Powerpipe Cloud workspace, which will be used as both the database and snapshot location (`snapshot_location`).



