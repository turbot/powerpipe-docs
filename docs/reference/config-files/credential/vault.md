---
title:  vault
sidebar_label: vault
---

# vault

The `vault` credential can be used to access Vault resources.

```hcl
credential "vault" "my_vault" {
  address = "http://127.0.0.1:8200"
  token = "hvs.FaKe"
}
```

## Arguments

| Name            | Type    | Required?| Description
|-----------------|---------|----------|-------------------
| `address`       |  String | Optional | Vault server address
| `token`         |  String | Optional | API token  


All arguments are optional, and a `vault` credential with no arguments will behave the same as the [default credential](#default-credential).  

## Default Credential
The `vault` credential type includes an implicit, default credential (`credential.vault.default`) that will be configured to set the `token` to the `VAULT_TOKEN` environment variable and the `domain` to `VAULT_ADDR`.

```hcl
credential "vault" "default" {
  domain = env("VAULT_ADDR")
  token  = env("VAULT_TOKEN")
}
```