---
title:  servicenow
sidebar_label: servicenow
---

# servicenow

The `servicenow` credential can be used to access ServiceNow resources.

```hcl
credential "servicenow" "snow_creds" {
  instance_url = "https://my-instance.service-now.com"
  username = "john.hill"
  password = "fake-p@ss"
}
```

## Arguments

| Name            | Type    | Required?| Description
|-----------------|---------|----------|-------------------
| `instance_url`  |  String | Optional | Your ServiceNow instance URL
| `username`      |  String | Optional | ServiceNow username  
| `password`      |  String | Optional | ServiceNow app password

All arguments are optional, and a `servicenow` credential with no arguments will behave the same as the [default credential](#default-credential).  

## Default Credential
The `servicenow` credential type includes an implicit, default credential (`credential.servicenow.default`) that will be configured using the environment variables `SERVICENOW_INSTANCE_URL`, `SERVICENOW_USERNAME`, and `SERVICENOW_PASSWORD`.

```hcl
credential "servicenow" "default" {
  instance_url = env("SERVICENOW_INSTANCE_URL")
  username = env("SERVICENOW_USERNAME")
  password = env("SERVICENOW_PASSWORD")
}
```