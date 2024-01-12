---
title: Powerpipe Server
sidebar_label: Powerpipe Server
---

# Powerpipe Server

By default, most Powerpipe commands run in client-only mode.  For example, `powerpipe pipeline run` will parse and load a mod from the current directory (or `--mod-location`), run the pipeline, print the results, and then exit. This makes it easy to run pipelines on an ad hoc basis - just run the command.

But what if you want to run a pipeline at regular intervals, or in response to an event?  Powerpipe [triggers](/docs/powerpipe-hcl/trigger/trigger) provide that capability.  To use triggers, you must run Powerpipe in server mode.  The `powerpipe server` command runs Powerpipe in server mode in the foreground: 

```bash
powerpipe server
```

Once it is running you can connect to from another terminal with the `--host` argument:
```bash
powerpipe pipeline list --host local
powerpipe pipeline run my_pipeline --host local
powerpipe trigger list --host local

```

Like all Powerpipe commands, `powerpipe server` will load the mod from the current directory by default, but you can specify a different directory with `--mod-location`:
```bash
powerpipe server --mod-location ~/my_powerpipe_mod
```

By default, Powerpipe will listen on all network interfaces, but you can pass `--listen local` if you only want to listen on the loopback addresses:
```bash
powerpipe server --listen local
```

Powerpipe listens on port 7103 by default, but you can use the `--port` argument to use a different one:
```bash
powerpipe server --port 7104
```

While the server is running, Powerpipe will watch your mod files for changes and automatically update the server.  You can disable this with the `--watch` argument if you prefer not to update the server instance as the files change:
```bash
powerpipe server --watch=false
```

Often it is simpler to manage all of these settings with a [workspace](/docs/run/workspaces) instead. For example, you can add a `workspace` to your workspaces file (`~/.powerpipe/config/workspaces.fpc`):

```hcl
workspace "my_server" {
  listen = local
  port   = 7104
  host   = http://localhost:7104
  watch  = false
}
```

And then use the `--workspace` flag when you start the server:
```bash
powerpipe server --workspace my_server
```

and when you run other Powerpipe commands against that server:
```bash
powerpipe trigger list --workspace my_server
```
