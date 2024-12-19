---
title: tailpipe
---

# tailpipe

```hcl
connection "tailpipe" "my_conn" {
  # Optional, defaults to "~/.tailpipe"
  install_dir = "/Users/cbruno/other_install_dir"

  # Optional, defaults to "default" workspace
  workspace = "my_workspace"

  # Optional, defaults to all partitions
  partitions = ["aws_cloudtrail_log.*", "github_audit_log.*"]

  # Optional, defaults to all indexes
  indexes = ["123456789012", "4567890123*", "turbot", "turbotio"]

  # Optional, if not set, beginning time range is unbound
  from = "2024-10-23T11:01:01"

  # Optional, if not set, ending time range is unbound
  to = "2024-11-01T11:01:01"
}
```


The `tailpipe` connection is used mainly to pre-filter the default database, scoping the records available to a mod by date range, index, or partition (subject to further interactive filtering). It can also be used to a non-default [Tailpipe](https://tailpipe.io/) database, useful when [[USE CASE]].


## Arguments

| Name         | Type    | Required?| Description
|--------------|---------|----------|-------------------
| `from`       |  String | Optional 
| `to`         |  String | Optional 
| `partition`  |  String | Optional 
| `index`      |  String | Optional 



