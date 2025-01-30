---
title: tailpipe
---

# tailpipe

```hcl
connection "tailpipe" "my_conn" {
  from        = "2024-10-23"
  to          = "2024-10-30"
}
```

The `tailpipe` connection is used to pre-filter the default database, scoping the records available to a mod by date range, index, or partition (subject to further interactive filtering). 


## Arguments

| Name         | Type    | Required?| Description
|--------------|---------|----------|-------------------
| `from`       |  String | Optional | Set the earliest date. Default: unbounded.
| `to`         |  String | Optional | Set the latest date. Default: unbounded.

## Default Connection

The `tailpipe` connection type includes an implicit, default connection (`connection.tailpipe.default`) that includes all collected data.

