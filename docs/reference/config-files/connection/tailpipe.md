---
title: tailpipe
---

> [!NOTE]
> this is speculation based on the wiki but not how things work as of 1/27

# tailpipe

```hcl
connection "tailpipe" "my_conn" {
  from        = "2024-10-23T11:01:01"
  partitions  = ["dev","prod"]
}
```

The `tailpipe` connection is used to pre-filter the default database, scoping the records available to a mod by date range, index, or partition (subject to further interactive filtering). 


## Arguments

| Name         | Type    | Required?| Description
|--------------|---------|----------|-------------------
| `install_dir`|  String | Optional | Tailpipe install location. Defaults to defaults to "~/.tailpipe".
| `from`       |  String | Optional | Set the earliest date. Default: unbounded.
| `to`         |  String | Optional | Set the latest date. Default: unbounded.
| `partitions  |  Array  | Optional | Include specific partitions. Default: all.
| `indexes`    |  Array  | Optional | Include specific indexes. Default: all.

## Attributes (Read-Only)

TBD

## Default Connection

TBD

