---
title: connection
---

# Connection

Powerpipe **Connections** provide a mechanism for defining credentials and options for interacting with external systems.

```hcl
connection "postgres" "my_server" {
   host     = "localhost"
   port     = 5432
   db       = "mydb"
   username = "myuser"
   password = "mypassword123"
}
```

Mod authors often use connections to [set the default database](/docs/build/mod-database), and allow users to [pass a connection at run time](/docs/run#selecting-a-database) using a variable.

Connections are defined using the `connection` block in one or more Powerpipe config files. Powerpipe will load ALL configuration files (`*.ppc`) from every directory in the [configuration search path](/docs/reference/env-vars/powerpipe_config_path), with decreasing precedence. The set of connections is the union of all connections defined in these directories.

Each connection has a type label and a name. There is a type for each service: [connection.steampipe](/docs/reference/config-files/connection/steampipe),  [connection.postgres](/docs/reference/config-files/connection/postgres), [connection.mysql](/docs/reference/config-files/connection/mysql), ,[connection.duckdb](/docs/reference/config-files/connection/duckdb), [connection.sqlite](/docs/reference/config-files/connection/sqlite), etc. The arguments and attributes vary by type.


### Default connections

Powerpipe also creates a single **implicit default** connection for each connection type. The connection is named `default` (`connection.steampipe.default`, `connection.postgres.default`, etc). The intent of the default connection is that Powerpipe can "just work" with your existing setup, and is similar to the default connection that Steampipe creates for each plugin. For example, the Postgres default connection will use the standard [libpq environment variables](https://www.postgresql.org/docs/current/libpq-envars.html) to create a Powerpipe connection that will use the same connection that the `psql` CLI would use, the Steampipe default connection will connect to the local Steampipe instance on `postgres://steampipe@127.0.0.1:9193/steampipe`, etc.

You can override the default by simply creating a connection for that type that is named `default`:

```hcl
connection "steampipe" "default" {
   host     = "steampipe.myorg.com"
   port     = 9193
   db       = "steampipe"
   username = "myusername"
   password = "mypassword123"
}
```
