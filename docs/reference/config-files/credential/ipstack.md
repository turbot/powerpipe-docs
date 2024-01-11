---
title:  ipstack
sidebar_label: ipstack
---

# ipstack

The `ipstack` credential can be used to access IpStack resources.

```hcl
credential "ipstack" "my_ipstack" {
  access_key = "1234801fakekeyf123455e6cfaf2"
}
```

## Arguments

| Name            | Type    | Required?| Description
|-----------------|---------|----------|-------------------
| `access_key`    |  String | Optional | API access key


All arguments are optional, and a `ipstack` credential with no arguments will behave the same as the [default credential](#default-credential).  

## Default Credential
The `ipstack` credential type includes an implicit, default credential (`credential.ipstack.default`) that will be configured to set the `access_key` to the `IPSTACK_ACCESS_KEY` environment variable.

```hcl
credential "ipstack" "default" {
  access_key = env("IPSTACK_ACCESS_KEY")
}
```