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
Powerpipe will load [configuration files](/docs/reference/config-files) (`*.ppc`) according to the configuration search path.  You can change this path with the `--config-path` argument or the [POWERPIPE_CONFIG_PATH](/docs/reference/env-vars/powerpipe_config_path) environment variable, but it defaults to `.:$POWERPIPE_INSTALL_DIR/config` (`.:~/.powerpipe/config`).  This allows you to manage your [workspaces](/docs/run/workspaces) centrally in the `~/.powerpipe/config` directory, but override them in the mod location if desired.


## Selecting a database

Most public mods are written without specifying a `database` on each query.  As a result, all queries in the mod run against the 'active' database. By default, the active database is `postgres://steampipe@localhost:9193/steampipe`, thus Powerpipe will run against a local Steampipe instance.  

You may instead run Powerpipe against a specific database by passing the `--database` argument.  This works with batch commands:

```bash
powerpipe benchmark run cis_v120 --database  postgres://myusername:passworrd@mydbserver.mydomain.com:9193/steampipe
```

and with the dashboard server:

```bash
powerpipe server --database  postgres://myusername:passworrd@mydbserver.mydomain.com:9193/steampipe
```

You may instead set the database with the [POWERPIPE_DATABASE](/docs/reference/env-vars/powerpipe_database) environment variable:

```bash
export POWERPIPE_DATABASE=postgres://myusername:passworrd@mydbserver.mydomain.com:9193/steampipe
powerpipe benchmark run cis_v120
powerpipe server

```

Or you can set it in a [workspace](/docs/run/workspaces) and then pass the workspace name to the command:
```bash
powerpipe benchmark run cis_v120 --workspace my_workspace
powerpipe server --workspace my_workspace

```

You can even change the default by [setting it in your `default` workspace](/docs/run/workspaces#using-workspaces).


If you use [Turbot Pipes](http://pipes.turbot.com), you can run Powerpipe against the Pipes workspace database (you will need to [log in](/docs/reference/cli/login) first):
```bash
powerpipe benchmark run cis_v120 --workspace acme/anvils
powerpipe server --workspace acme/anvils

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

## Targeting specific schemas/connections (Postgres/Steampipe)

A PostgreSQL database contains one or more [schemas](https://www.postgresql.org/docs/current/ddl-schemas.html). A schema is a namespaced collection of named objects, like tables, functions, and views.   When writing a query, you may qualify the table name with the schema name (e.g. `select * from schema_name.table_name`) or use an unqualified name (`select * from table_name`).  When using unqualified names, the system determines which table is meant by following a [search path](https://www.postgresql.org/docs/current/ddl-schemas.html#DDL-SCHEMAS-PATH); the first matching table in the search path is taken to be the one wanted. 

Usually, Powerpipe mods use unqualified queries to "target" whichever connection is first in the [search path](https://steampipe.io/docs/guides/search-path), but you can specify a different path if you want:

```bash
powerpipe benchmark run cis_v150 --search-path aws_connection_2,github,slack
```

The `--search-path` argument will replace the entire search path.  Often you just want to move a single connection to the front of the path:

```bash
powerpipe benchmark run cis_v150 --search-path-prefix aws_connection_2
```

[Steampipe](https://steampipe.io) creates a schema for each connection and aggregator, and understanding how the search path works is important when using Steampipe and Powerpipe. See the [Using search_path to target connections and aggregators](https://steampipe.io/docs/guides/search-path) guide for more information.


## Operating Modes

Powerpipe can operate in 2 modes.

By default, Powerpipe runs in **Client-only Mode**.  Powerpipe loads the mod, runs the command, and exits.  [Interactive dashboards](/docs/run/dashboard) are not enabled in Client-Only Mode.

If you run Powerpipe in **Server Mode** mode, Powerpipe will run a dashboard server (on port `9033` by default).  In [this mode](/docs/run/server), you can browse [Interactive dashboards](/docs/run/dashboard) by navigating to `http://localhost:9033/` in your web browser.