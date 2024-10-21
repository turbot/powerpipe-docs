---
title:  postgres
sidebar_label: postgres
---

# postgres

The `postgres` connection can be used to access a PostgreSQL database.

```hcl
connection "postgres" "my_connection" {
   host     = "localhost"
   port     = 5432
   db       = "mydb"
   username = "myuser"
   password = "mypassword123"
}
```

## Arguments

| Name                | Type    | Required?| Description
|---------------------|---------|----------|-------------------
| `db`                |  String | Optional | Database name.  Defaults to `$PGDATABASE`, or `postgres` if unset.
| `host`              |  String | Optional | Database hostname.  Defaults to `$PGHOST` or `localhost` if not set.
| `password`          |  String | Optional | Database password. Defaults to `$PGPASSWORD`
| `port`              |  Number | Optional | Database port.  Defaults to `$PGPORT` or `5432` if not set.
| `search_path`       |  String | Optional | Database search path.
| `search_path_prefix`|  String | Optional | Database search path prefix.
| `ssl_mode`          |  String | Optional | PostgreSQL [SSL Mode](https://www.postgresql.org/docs/current/libpq-ssl.html#LIBPQ-SSL-PROTECTION), one of  `disable`, `allow`, `prefer`, `require`, `verify-ca`, `verify-full`.  The defaults to `$PGSSLNEGOTIATION`.
| `username`          |  String | Optional |  Database username. Defaults to `$PGUSER` or `postgres` if not set.


All arguments are optional, and a `postgres` connection with no arguments will behave the same as the [default connection](#default-connection).


## Attributes (Read-Only)

| Attribute           | Type   | Description
| --------------------| ------ |------------------------------------------------------------------------------
| `connection_string` | String | The connection string built from the arguments to this connection, in the format `postgresql://[username[:password]@][host][:port][/db]`
| `env`               | Map    | A map of the resolved [libpq environment variables](https://www.postgresql.org/docs/current/libpq-envars.html) (`PGHOST`, `PGDATABASE`, `PGUSER`, `PGPASSWORD`, `PGPORT`, `PGSSLNEGOTIATION`)



## Default Connection

The `postgres` connection type includes an implicit, default connection (`connection.postgres.default`) that will be configured using the [libpq environment variables](https://www.postgresql.org/docs/current/libpq-envars.html).


```hcl
connection "postgres" "default" {
  db       = env("PGDATABASE")
  host     = env("PGHOST")
  password = env("PGPASSWORD")
  port     = env("PGPORT")
  ssl_mode = env("PGSSLNEGOTIATION")
  username = env("PGUSER")
}
```