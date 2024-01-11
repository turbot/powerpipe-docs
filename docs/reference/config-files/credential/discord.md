---
title:  discord
sidebar_label: discord
---

# discord

The `discord` credential can be used to access Discord resources.

```hcl
credential "discord" "my_discord" {
  token = "00B630jSCGU4Fake5Yh4KQMFakezwE2OgVcS7N999b"
}
```

## Arguments

| Name            | Type    | Required?| Description
|-----------------|---------|----------|-------------------
| `token`         |  String | Optional | API token 


All arguments are optional, and a `discord` credential with no arguments will behave the same as the [default credential](#default-credential).  

## Default Credential
The `discord` credential type includes an implicit, default credential (`credential.discord.default`) that will be configured to set the `token` to the `DISCORD_TOKEN` environment variable.

```hcl
credential "discord" "default" {
  token = env("DISCORD_TOKEN")
}
```
