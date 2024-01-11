---
title:  ip2locationio
sidebar_label: ip2locationio
---

# ip2locationio

The `ip2locationio` credential can be used to access Ip2Location.io resources.

```hcl
credential "ip2locationio" "my_api_key" {
  api_key = "12345678901A23BC4D5E6FG78HI9J101"
}
```

## Arguments

| Name            | Type    | Required?| Description
|-----------------|---------|----------|-------------------
| `api_key`       |  String | Optional | API key  


All arguments are optional, and a `ip2locationio` credential with no arguments will behave the same as the [default credential](#default-credential).  

## Default Credential
The `ip2locationio` credential type includes an implicit, default credential (`credential.ip2locationio.default`) that will be configured to set the `api_key` to the `IP2LOCATIONIO_API_KEY` environment variable.

```hcl
credential "ip2locationio" "default" {
  api_key = env("IP2LOCATIONIO_API_KEY")
}
```
