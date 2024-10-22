---
title:  mysql
sidebar_label: mysql
---

# mysql

The `mysql` connection can be used to access a MySQL database.

```hcl
connection "mysql" "my_connection" {
   host     = "localhost"
   port     = 3306
   db       = "mydb"
   username = "myuser"
   password = "mypassword123"
}
```

## Arguments

| Name                | Type    | Required?| Description
|---------------------|---------|----------|-------------------
| `db`                |  String | Optional | Database name.  Defaults to `mysql`.
| `host`              |  String | Optional | Database hostname.  Defaults to `localhost`.
| `password`          |  String | Optional | Database password.
| `port`              |  Number | Optional | Database port.  Defaults to `3306`.
| `username`          |  String | Optional |  Database username. Defaults to `root`.


All arguments are optional, and a `mysql` connection with no arguments will behave the same as the [default connection](#default-connection).


## Attributes (Read-Only)

| Attribute           | Type   | Description
| --------------------| ------ |------------------------------------------------------------------------------
| `connection_string` | String | The connection string built from the arguments to this connection, in the format `mysql://[username[:password]@][host][:port][/db]`




## Default Connection

The `mysql` connection type includes an implicit, default connection (`connection.mysql.default`) that will be configured using the following default values:

```hcl
connection "mysql" "default" {
  db       = "mysql"
  host     = "localhost"
  password = ""
  port     = 3306
  username = "root"
}
```