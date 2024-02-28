---
title: Run Powerpipe
sidebar_label: Run Powerpipe
---

# Run Powerpipe

Powerpipe is simple to install and manage and does not require any special expertise to get started.  It's distributed as a single binary file - just [download and run it](/downloads)!

## Mod Location
Powerpipe always runs in the context of a [mod](/docs/build), which is a collection of Powerpipe benchmarks, controls, dashboards, and queries.  You can [import and use resources from other mods](/docs/build/mod-dependencies) so you can get started without even writing any code! You can explore the available mods on the [Powerpipe Hub](https://hub.powerpipe.io/).

Powerpipe loads the mod from the current directory by default, but you can pass the [--mod-location](/docs/reference/cli#global-flags) flag or set the [POWERPIPE_MOD_LOCATION](/docs/reference/env-vars/powerpipe_mod_location) to set it to a different path.  

## Configuration Files
Powerpipe will load [configuration files](/docs/reference/config-files) (`*.ppc`) according to the configuration search path.  You can change this path with the `--config-path` argument or the [POWERPIPE_CONFIG_PATH](/docs/reference/env-vars/powerpipe_config_path) environment variable, but it defaults to `.:$POWERPIPE_INSTALL_DIR/config` (`.:~/.powerpipe/config`).  This allows you to manage your [workspaces](/docs/run/workspaces) centrally in the `~/.powerpipe/config` directory, but override them in the working directory / mod location if desired.


## Selecting a database

Most public mods are written without specifying a `database` on each query.  As a result, all queries in the mod run against the 'active'' database. By default, the active database is `postgres://steampipe@localhost:9193/steampipe`, thus Powerpipe will run against a local Steampipe instance.  

You may instead run a benchmark or control against a specific database by passing the `--database` argument:
```bash
powerpipe benchmark run cis_v120 --database  postgres://myusername:passworrd@mydbserver.mydomain.com:9193/steampipe
```

Or setting the [POWERPIPE_DATABASE](/docs/reference/env-vars/powerpipe_database) environment variable:

```bash
export POWERPIPE_DATABASE=postgres://myusername:passworrd@mydbserver.mydomain.com:9193/steampipe
powerpipe benchmark run cis_v120
```

You can also set it in a [workspace](/docs/run/workspaces) and then pass the workspace name to the command:
```bash
powerpipe benchmark run cis_v120 --workspace my_workspace
```

You can even change the default by [setting it in your `default` workspace](/docs/run/workspaces#using-workspaces).

If you use [Turbot Pipes](http://pipes.turbot.com), you can run a benchmark against the pipes workspace by name (you will need to [login](/docs/reference/cli/login) first):
```bash
powerpipe benchmark run cis_v120 --workspace acme/anvils
```

While most Powerpipe mods are written for Steampipe databases, Powerpipe can also connect to other engines, including Postgres:

```bash
powerpipe server --database 'postgresql://myusername:mypassword@acme-prod.apse1.db.cloud.turbot.io:9193/aaa000'
```

MySQL:

```bash
powerpipe server --database 'mysql://root:my_pass@tcp(localhost)/mysql'
```

SQLite:
```bash
powerpipe server --database 'sqlite:./my_sqlite_db.db'
```

and DuckDB :
```bash
powerpipe server --database 'duckdb:./my_ducks.db'
```


## Operating Modes

Powerpipe can operate in 2 modes.

By default, Powerpipe runs in **Client-only Mode**.  Powerpipe loads the mod, runs the command, and exits.  [Interactive dashboards](/docs/run/dashboard) are not enabled in Client-Only Mode.

If you run Powerpipe in **Server Mode** mode, Powerpipe will run an API server (on port `9033` by default).  In [this mode](/docs/run/server), you can browse [Interactive dashboards](/docs/run/dashboard) by navigating to `http://localhost:9033/` in your web browser.  After you start the Powerpipe server, you can run Powerpipe commands against it by specifying the [--host](/docs/reference/cli#global-flags) argument.
