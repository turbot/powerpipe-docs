---
title:  steampipe
sidebar_label: steampipe
---

# steampipe

The `steampipe` connection can be used to access a [Steampipe](https://steampipe.io/) database.

```hcl
connection "steampipe" "steampipe_connection" {
   host     = "localhost"
   port     = 9193
   db       = "steampipe"
   username = "steampipe"
   password = "mypassword123"
}
```

## Arguments

| Name                | Type    | Required?| Description
|---------------------|---------|----------|-------------------
| `db`                |  String | Optional | Database name.  Defaults to `steampipe`.
| `host`              |  String | Optional | Database hostname.  Defaults to `127.0.0.1`.
| `password`          |  String | Optional | Database password.
| `port`              |  Number | Optional | Database port.  Defaults to `9193`.
| `search_path`       |  String | Optional | Database search path.
| `search_path_prefix`|  String | Optional | Database search path prefix.
| `ssl_mode`          |  String | Optional | PostgreSQL [SSL Mode](https://www.postgresql.org/docs/current/libpq-ssl.html#LIBPQ-SSL-PROTECTION), one of `disable`, `allow`, `prefer`, `require`, `verify-ca`, `verify-full`.  The default is `require`.
| `username`          |  String | Optional |  Database username. Default is `steampipe`.


All arguments are optional, and a `steampipe` connection with no arguments will behave the same as the [default connection](#default-connection).


## Attributes (Read-Only)

| Attribute           | Type   | Description
| --------------------| ------ |------------------------------------------------------------------------------
| `connection_string` | String | The connection string built from the arguments to this connection, in the format `postgresql://[username[:password]@][host][:port][/db]`
| `env`               | Map    | A map of the resolved [libpq environment variables](https://www.postgresql.org/docs/current/libpq-envars.html) (`PGHOST`, `PGDATABASE`, `PGUSER`, `PGPASSWORD`, `PGPORT`, `PGSSLNEGOTIATION`)



## Default Connection

The `steampipe` connection type includes an implicit, default connection (`connection.steampipe.default`) that will be configured to use the local Steampipe instance, eg:

```hcl
   host     = "localhost"
   port     = 9193
   db       = "steampipe"
   username = "steampipe"
```