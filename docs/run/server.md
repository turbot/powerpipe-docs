---
title: Powerpipe Server
sidebar_label: Powerpipe Server
---

# Powerpipe Server

By default, most Powerpipe commands run in client-only mode.  For example, `powerpipe benchmark run` will parse and load a mod from the current directory (or `--mod-location`), run the benchmark, print the results, and then exit. This makes it easy to run [benchmarks and controls](/docs/run/benchmark) on an ad hoc basis - just run the command.


But Powerpipe also allows you to browse and view interactive [dashboards](/docs/run/dashboard).  The `powerpipe server` command runs Powerpipe in server mode in the foreground:

```bash
powerpipe server
```

Once it is running you can view the the dashboards in your web browser by navigating to `http://locahost:9033`.


Like all Powerpipe commands, `powerpipe server` will load the mod from the current directory by default, but you can specify a different directory with `--mod-location`:

```bash
powerpipe server --mod-location ~/my_powerpipe_mod
```

By default, Powerpipe will listen on all network interfaces, but you can pass `--listen local` if you only want to listen on the loopback addresses:

```bash
powerpipe server --listen local
```

Powerpipe listens on port 9033 by default, but you can use the `--port` argument to use a different one:

```bash
powerpipe server --port 9034
```

While the server is running, Powerpipe will watch your mod files for changes and automatically update the server.  You can disable this with the `--watch` argument if you prefer not to update the server instance as the files change:

```bash
powerpipe server --watch=false
```

Often it is simpler to manage all of these settings with a [workspace](/docs/run/workspaces) instead. For example, you can change the default values by editing your [default workspace](/docs/run/workspaces#using-workspaces):

```hcl
workspace "default" {
  listen = "local"
  port   = 9034
  watch  = false
}
```