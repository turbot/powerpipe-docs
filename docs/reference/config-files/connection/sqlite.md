---
title:  sqlite
sidebar_label: sqlite
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
<!--


---
title:  sqlite
sidebar_label: sqlite
---


# sqlite

The `sqlite` connection can be used to access a [SQLite](https://www.sqlite.org/) database.

```hcl
connection "sqlite" "sqlite_connection" {
   connection_string = "sqlite://my_sqlite_db.db"
}
```

## Arguments

| Name                | Type    | Required?| Description
|---------------------|---------|----------|-------------------
| `connection_string` |  String | Optional | SQLite connection string



### Connection String

The SQLite connection string is the path to a SQLite database file:

```bash
sqlite:path/to/file
```

The path is relative to the [mod location](/docs/run#mod-location), and `//` is optional after the scheme, thus the following are equivalent relative paths:

```hcl
sqlite:./my_sqlite_db.db
sqlite://./my_sqlite_db.db
sqlite://my_sqlite_db.db
```

and these are equivalent absolute paths:

```hcl
sqlite:/var/db/my_sqlite_db.db
sqlite:///var/db/my_sqlite_db.db
```

All arguments are optional, and a `sqlite` connection with no arguments will behave the same as the [default connection](#default-connection).


## Default Connection

The `sqlite` connection type includes an implicit, default connection (`connection.sqlite.default`) that will be configured using the environment variables...
```hcl
connection "sqlite" "default" {

}
```


-->