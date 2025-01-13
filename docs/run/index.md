---
title: Run Powerpipe
---

>[!NOTE]
> how to include tailpipe here? add a database-oriented section for tp?


# Run Powerpipe

Powerpipe is simple to install and manage and does not require any special expertise to get started.  It's distributed as a single binary file - just [download and run it](/downloads)!

## Mod Location
Powerpipe always runs in the context of a [mod](/docs/build), which is a collection of Powerpipe benchmarks, controls, dashboards, and queries.  You can [import and use resources from other mods](/docs/build/mod-dependencies) so you can get started without even writing any code! You can explore the available mods on the [Powerpipe Hub](https://hub.powerpipe.io/).

Powerpipe loads the mod from the current directory by default, but you can pass the [--mod-location](/docs/reference/cli#global-flags) flag or set the [POWERPIPE_MOD_LOCATION](/docs/reference/env-vars/powerpipe_mod_location) to set it to a different path.  

## Configuration Files
Powerpipe will load [configuration files](/docs/reference/config-files) (`*.ppc`) according to the configuration search path.  You can change this path with the `--config-path` argument or the [POWERPIPE_CONFIG_PATH](/docs/reference/env-vars/powerpipe_config_path) environment variable, but it defaults to `.:$POWERPIPE_INSTALL_DIR/config` (`.:~/.powerpipe/config`).  This allows you to manage your [workspaces](/docs/run/workspaces) centrally in the `~/.powerpipe/config` directory, but override them in the mod location if desired.

## Operating Modes

Powerpipe can operate in 2 modes.

By default, Powerpipe runs in **Client-only Mode**.  Powerpipe loads the mod, runs the command, and exits.  [Interactive dashboards](/docs/run/dashboard) are not enabled in Client-Only Mode.

If you run Powerpipe in **Server Mode** mode, Powerpipe will run a dashboard server (on port `9033` by default).  In [this mode](/docs/run/server), you can browse [Interactive dashboards](/docs/run/dashboard) by navigating to `http://localhost:9033/` in your web browser.

## Selecting a database

### Steampipe

Most public mods are written without specifying a `database` on each query.  As a result, all queries in the mod run against the 'active' database. By default, the active database is `postgres://steampipe@localhost:9193/steampipe`, thus Powerpipe will run against a local Steampipe instance.

Some mods allow you to [set the database via a variable](/docs/build/mod-database). In this case, you can pass the connection to the variable when you run a powerpipe command:

```bash
powerpipe benchmark run cis_v120 --var database=connection.steampipe.my_connection
```

or start the dashboard server:

```bash
powerpipe server --var database=connection.steampipe.my_connection
```

For the database connection types ([connection.steampipe](/docs/reference/config-files/connection/steampipe),  [connection.postgres](/docs/reference/config-files/connection/postgres), [connection.mysql](/docs/reference/config-files/connection/mysql), [connection.duckdb](/docs/reference/config-files/connection/duckdb), [connection.sqlite](/docs/reference/config-files/connection/sqlite)), Powerpipe even supports passing them as connection strings:

```bash
powerpipe benchmark run cis_v120 --var database="postgres://steampipe@127.0.0.1:9193/steampipe"
```

or pipes workspaces (you will need to [log in](/docs/reference/cli/login) first)!
```bash
powerpipe benchmark run cis_v120 --var database=tnt/fireworks
```

### Targeting specific Postgres/Steampipe schemas/connections 

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


### Tailpipe

????