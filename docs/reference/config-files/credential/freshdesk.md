---
title:  freshdesk
sidebar_label: freshdesk
---

# freshdesk

The `freshdesk` credential can be used to access Freshdesk resources.

```hcl
credential "freshdesk" "freshdesk_creds" {
  api_key = "sk-jwgthNa..."
  subdomain = "domain_name"
}
```

## Arguments

| Name            | Type    | Required?| Description
|-----------------|---------|----------|-------------------
| `api_key`       |  String | Optional | Freshdesk API key  
| `subdomain`     |  String | Optional | Freshdesk subdomain

All arguments are optional, and a `freshdesk` credential with no arguments will behave the same as the [default credential](#default-credential).  

## Default Credential
The `freshdesk` credential type includes an implicit, default credential (`credential.freshdesk.default`) that will be configured using the environment variables `FRESHDESK_API_KEY` and `FRESHDESK_SUBDOMAIN`.

```hcl
credential "freshdesk" "default" {
  api_key = env("FRESHDESK_API_KEY")
  subdomain = env("FRESHDESK_SUBDOMAIN")
}
```