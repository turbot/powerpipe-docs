---
title:  jumpcloud
sidebar_label: jumpcloud
---

# jumpcloud

The `jumpcloud` credential can be used to access JumpCloud resources.

```hcl
credential "jumpcloud" "jumpcloud_api_key" {
  api_key = "2e256f0faked412345676fake914cf7e68d8e53c8d"
}
```

## Arguments

| Name            | Type    | Required?| Description
|-----------------|---------|----------|-------------------
| `api_key`       |  String | Optional | API Key


All arguments are optional, and an `jumpcloud` credential with no arguments will behave the same as the [default credential](#default-credential).

## Default Credential
The `jumpcloud` credential type includes an implicit, default credential (`credential.jumpcloud.default`) that will be configured to set the `api_key` to the `JUMPCLOUD_API_KEY` environment variable.

```hcl
credential "jumpcloud" "default" {
  api_key = env("JUMPCLOUD_API_KEY")
}
```