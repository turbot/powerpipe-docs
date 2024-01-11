---
title:  zendesk
sidebar_label: zendesk
---

# zendesk

The `zendesk` credential can be used to access Zendesk resources.

```hcl
credential "zendesk" "my_zendesk" {
  subdomain  = "dmi"
  email      = "pam@dmi.com"
  token      = "17ImlCYdfThisIsFakepJn1fi1pLwVdrb21114"
}
```

## Arguments

| Name            | Type    | Required?| Description
|-----------------|---------|----------|-------------------
| `subdomain`     |  String | Optional | The subdomain name of your Zendesk account.
| `email`         |  String | Optional | Email address of agent user who have permission to access the API.  
| `token`         |  String | Optional | API Token for your Zendesk instance.


All arguments are optional, and a `zendesk` credential with no arguments will behave the same as the [default credential](#default-credential).  

## Default Credential
The `zendesk` credential type includes an implicit, default credential (`credential.zendesk.default`) that will be configured to set defaults using the environment variables `ZENDESK_EMAIL`, `ZENDESK_SUBDOMAIN`, and `ZENDESK_API_TOKEN`.

```hcl
credential "zendesk" "default" {
  subdomain  = env("ZENDESK_SUBDOMAIN")
  email      = env("ZENDESK_EMAIL")
  token      = env("ZENDESK_API_TOKEN")
}
```