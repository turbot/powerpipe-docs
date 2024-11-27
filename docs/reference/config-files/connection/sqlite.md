---
title: sqlite
---


# sqlite

The `sqlite` connection can be used to access a [SQLite](https://www.sqlite.org/) database.

```hcl
connection "sqlite" "sqlite_connection" {
   filename = "./my_sqlite_db.db"
}
```


## Arguments

| Name       | Type    | Required?| Description
|------------|---------|----------|-------------------
| `filename` |  String | Optional | Path to a SQLite database file to open. The filename is relative to the [mod location](/docs/run#mod-location)

All arguments are optional, and a `sqlite` connection with no arguments will behave the same as the [default connection](#default-connection).


## Attributes (Read-Only)

| Attribute           | Type   | Description
| --------------------| ------ |------------------------------------------------------------------------------
| `connection_string` | String | The connection string built from the arguments to this connection, in the format `sqlite://path/to/file`



## Default Connection

The `sqlite` connection type includes an implicit, default connection (`connection.sqlite.default`), which will use the `SQLITE_FILENAME` environment variable unless overridden.

```hcl
connection "sqlite" "default" {
   file = env("SQLITE_FILENAME")
}
```
