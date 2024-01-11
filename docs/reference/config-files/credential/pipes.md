---
title:  pipes
sidebar_label: pipes
---

# pipes

The `pipes` credential can be used to access [Turbot Pipes](https://pipes.turbot.com/) resources.

```hcl
credential "pipes" "my_pipes" {
  token = "tpt_cld630jTisISAFakeTokenAdqizwE2OgVcS7N9UHb"
}
```

## Arguments

| Name            | Type    | Required?| Description
|-----------------|---------|----------|-------------------
| `token`         |  String | Optional | API token 


All arguments are optional, and a `pipes` credential with no arguments will behave the same as the [default credential](#default-credential).  

## Default Credential
The `pipes` credential type includes an implicit, default credential (`credential.pipes.default`) that will be configured to set the `token` to the `PIPES_TOKEN` environment variable.

```hcl
credential "pipes" "default" {
  token = env("PIPES_TOKEN")
}
```