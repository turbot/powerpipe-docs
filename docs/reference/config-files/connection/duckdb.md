---
title:  duckdb
---

# duckdb

The `duckdb` connection can be used to access a [DuckDB](https://duckdb.org/) database.

```hcl
connection "duckdb" "duckdb_connection" {
   filename = "my_ducks.db"
}
```

## Arguments

| Name       | Type    | Required?| Description
|------------|---------|----------|-------------------
| `filename` |  String | Optional | Path to a DuckDB database file to open. The filename is relative to the [mod location](/docs/run#mod-location)


All arguments are optional, and a `duckdb` connection with no arguments will behave the same as the [default connection](#default-connection).

## Attributes (Read-Only)

| Attribute           | Type   | Description
| --------------------| ------ |------------------------------------------------------------------------------
| `connection_string` | String | The connection string built from the arguments to this connection, in the format `duckdb://path/to/file`



## Default Connection

The `duckdb` connection type includes an implicit, default connection (`connection.duckdb.default`), which will use the `DUCKDB_FILENAME` environment variable unless overridden.

```hcl
connection "duckcb" "default" {
   filename = env("DUCKDB_FILENAME")
}
```
