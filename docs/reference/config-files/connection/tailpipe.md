---
title: tailpipe
---

# tailpipe

```hcl
connection "tailpipe" "my_conn" {
  from      = "2024-10-23T11:01:01"
  partition = ["dev","prod"]
}
```

The `tailpipe` connection is used to pre-filter the default database, scoping the records available to a mod by date range, index, or partition (subject to further interactive filtering). 


## Arguments

| Name         | Type    | Required?| Description
|--------------|---------|----------|-------------------
| `install_dir`|  String | Optional | Tailpipe install location. Defaults to defaults to "~/.tailpipe".
| `from`       |  String | Optional | Set the earliest date. Default: unbounded.
| `to`         |  String | Optional | Set the latest date. Default: unbounded.
| `partition`  |  Array  | Optional | Include specific partions. Default: all.
| `index`      |  Array  | Optional | Include specific indexes. Default: all.

## Attributes (Read-Only)

| Attribute           | Type   | Description
| --------------------| ------ |------------------------------------------------------------------------------
| `connection_string` | String | The connection string built from the arguments to this connection, in the format `duckdb://path/to/file`


## Default Connection

The `tailpipe` connection type includes an implicit, default connection (`connection.tailpipe.default`), which will use the `TAILPIPE_FILENAME` environment variable unless overridden.

```hcl
connection "tailpipe" "default" {
   file = env("TAILPIPE_FILENAME")
}
```

