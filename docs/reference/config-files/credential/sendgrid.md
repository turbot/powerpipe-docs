---
title:  sendgrid
sidebar_label: sendgrid
---

# sendgrid

The `sendgrid` credential can be used to access SendGrid resources.

```hcl
credential "sendgrid" "my_sendgrid" {
  api_key = "SG.R7..."
}
```
## Arguments

| Name            | Type    | Required?| Description
|-----------------|---------|----------|-------------------
| `api_key`       |  String | Optional | SendGrid API Key  


All arguments are optional, and a `sendgrid` credential with no arguments will behave the same as the [default credential](#default-credential).  

## Default Credential
The `sendgrid` credential type includes an implicit, default credential (`credential.sendgrid.default`) that will be configured to set the `api_key` to the `SENDGRID_API_KEY` environment variable.

```hcl
credential "sendgrid" "default" {
  api_key = env("SENDGRID_API_KEY")
}
```