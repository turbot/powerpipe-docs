---
title:  uptimerobot
sidebar_label: uptimerobot
---

# uptimerobot

The `uptimerobot` credential can be used to access UptimeRobot resources.

```hcl
credential "uptimerobot" "my_uptimerobot" {
  api_key = "u123456-ecaf32fakekey633ff33dd3c445"
}
```

## Arguments

| Name            | Type    | Required?| Description
|-----------------|---------|----------|-------------------
| `api_key`       |  String | Optional | API Key


All arguments are optional, and a `uptimerobot` credential with no arguments will behave the same as the [default credential](#default-credential).  

## Default Credential
The `uptimerobot` credential type includes an implicit, default credential (`credential.uptimerobot.default`) that will be configured to set the `api_key` to the `UPTIMEROBOT_API_KEY` environment variable.

```hcl
credential "uptimerobot" "default" {
  api_key = env("UPTIMEROBOT_API_KEY")
}
```