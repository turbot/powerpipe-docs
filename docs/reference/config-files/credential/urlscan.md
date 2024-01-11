---
title:  urlscan
sidebar_label: urlscan
---

# urlscan

The `urlscan` credential can be used to access URLScan resources.

```hcl
credential "urlscan" "my_urlscan" {
  api_key = "11111111-e127-1234-adcd-59cad2f12abc"
}
```

## Arguments

| Name            | Type    | Required?| Description
|-----------------|---------|----------|-------------------
| `api_key`       |  String | Optional | API   


All arguments are optional, and a `urlscan` credential with no arguments will behave the same as the [default credential](#default-credential).  

## Default Credential

The `urlscan` credential type includes an implicit, default credential (`credential.urlscan.default`) that will be configured to set the `api_key` to the `URLSCAN_API_KEY` environment variable.

```hcl
credential "urlscan" "default" {
  api_key = env("URLSCAN_API_KEY")
}
```