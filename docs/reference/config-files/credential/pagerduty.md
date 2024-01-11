---
title:  pagerduty
sidebar_label: pagerduty
---

# pagerduty

The `pagerduty` credential can be used to access PagerDuty resources.

```hcl
credential "pagerduty" "my_pagerduty" {
  token = "u+_szhLR4FakeTokenGZSA"
}
```

## Arguments

| Name            | Type    | Required?| Description
|-----------------|---------|----------|-------------------
| `token`         |  String | Optional | API token


All arguments are optional, and a `pagerduty` credential with no arguments will behave the same as the [default credential](#default-credential).  

## Default Credential
The `pagerduty` credential type includes an implicit, default credential (`credential.pagerduty.default`) that will be configured to set the `token` to the `PAGERDUTY_TOKEN` environment variable.

```hcl
credential "pagerduty" "default" {
  token = env("PAGERDUTY_TOKEN")
}
```