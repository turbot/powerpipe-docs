---
title: Setting the Database
---

# Setting the Database

Most public mods are written without specifying a `database` on each query.  As a result, all queries in the mod run against the 'default' database. By default, the database is local Steampipe instance (`postgres://steampipe@localhost:9193/steampipe`).  The reason for this is partly historical; dashboards and benchmarks were originally built-in to Steampipe.  When we separated these features into separate products, we did not want to break compatibility with the large library of [existing mods](https://hub.powerpipe.io/).

It is possible, however, to set a different default database for your mod.  To do so, you can set the `database` argument in your [mod definition](/docs/powerpipe-hcl/mod).


```hcl
mod "my_mod" { 
  title          = "my mod"
  database       = "postgres://me:ThisIsATerriblePassword@1.2.3.4:5432:mydb"
}
```

The `database` can be a connection string for a Postgres database, but Powerpipe also supports MySQL:

```hcl
mod "my_mod" { 
  title          = "my mod"
  database       = "mysql://root:my_pass@tcp(localhost)/mysql"
}
```


and SQLite:

```hcl
mod "my_mod" { 
  title          = "my mod"
  database       = "sqlite:./my_sqlite_db.db"
}
```


and DuckDB:

```hcl
mod "my_mod" { 
  title          = "my mod"
  database       = "duckdb:./my_ducks.db"
}
```

You probably don't want to hardcode the connection string in your mod though. The connection string may include your database password, and you probably want to let the user choose which database to use at run time.  The preferred approach is a to use a variable to set the `database`, and use a [connection](/docs/reference/config-files/connection/) to set the default instead of the actual connection string.

```hcl
variable "database" {
  type        = connection.steampipe
  description = "Steampipe database connection string."
  default     = connection.steampipe.default
}

mod "my_mod" { 
  title          = "my mod"
  database       = var.database
}
```

Now when a user runs your mod they can either use the default connection:
```bash
powerpipe benchmark run cis_v120
```
or they can pass the variable to override it:

```bash
powerpipe benchmark run cis_v120 --var database=connection.steampipe.my_other_connection
```


For the database connection types ([connection.steampipe](/docs/reference/config-files/connection/steampipe), [connection.postgres](/docs/reference/config-files/connection/postgres), [connection.mysql](/docs/reference/config-files/connection/mysql), [connection.duckdb](/docs/reference/config-files/connection/duckdb), [connection.sqlite](/docs/reference/config-files/connection/sqlite)), Powerpipe even supports passing them as connection strings:

```bash
powerpipe benchmark run cis_v120 --var database="postgres://steampipe@127.0.0.1:9193/steampipe"
```

or pipes workspaces (you will need to [log in](/docs/reference/cli/login) first)!
```bash
powerpipe benchmark run cis_v120 --var database=tnt/fireworks
```