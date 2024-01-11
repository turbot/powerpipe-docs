---
title: Flowpipe Server
sidebar_label: Flowpipe Server
---

# Flowpipe Server

By default, most Flowpipe commands run in client-only mode.  For example, `flowpipe pipeline run` will parse and load a mod from the current directory (or `--mod-location`), run the pipeline, print the results, and then exit. This makes it easy to run pipelines on an ad hoc basis - just run the command.

But what if you want to run a pipeline at regular intervals, or in response to an event?  Flowpipe [triggers](/docs/flowpipe-hcl/trigger/trigger) provide that capability.  To use triggers, you must run Flowpipe in server mode.  The `flowpipe server` command runs Flowpipe in server mode in the foreground: 

```bash
flowpipe server
```

Once it is running you can connect to from another terminal with the `--host` argument:
```bash
flowpipe pipeline list --host local
flowpipe pipeline run my_pipeline --host local
flowpipe trigger list --host local

```

Like all Flowpipe commands, `flowpipe server` will load the mod from the current directory by default, but you can specify a different directory with `--mod-location`:
```bash
flowpipe server --mod-location ~/my_flowpipe_mod
```

By default, Flowpipe will listen on all network interfaces, but you can pass `--listen local` if you only want to listen on the loopback addresses:
```bash
flowpipe server --listen local
```

Flowpipe listens on port 7103 by default, but you can use the `--port` argument to use a different one:
```bash
flowpipe server --port 7104
```

While the server is running, Flowpipe will watch your mod files for changes and automatically update the server.  You can disable this with the `--watch` argument if you prefer not to update the server instance as the files change:
```bash
flowpipe server --watch=false
```

Often it is simpler to manage all of these settings with a [workspace](/docs/run/workspaces) instead. For example, you can add a `workspace` to your workspaces file (`~/.flowpipe/config/workspaces.fpc`):

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
flowpipe server --workspace my_server
```

and when you run other Flowpipe commands against that server:
```bash
flowpipe trigger list --workspace my_server
```
