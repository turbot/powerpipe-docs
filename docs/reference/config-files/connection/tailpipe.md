---
title: tailpipe
---

# tailpipe

```hcl
connection "tailpipe" "my_connection" {
   x = y
   y = z
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


