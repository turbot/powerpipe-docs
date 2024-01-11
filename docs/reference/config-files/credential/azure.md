---
title:  azure
sidebar_label: azure
---

# azure

The `azure` credential can be used to access Azure resources.

```hcl
credential "azure" "azure_creds" {
  tenant_id       = "YourTenantID"
  client_secret   = "YourClientSecret"
  client_id       = "YourClientID"
}
```

## Arguments

| Name            | Type    | Required?| Description
|-----------------|---------|----------|-------------------
| `tenant_id`     |  String | Optional | The Microsoft Entra tenant (directory) ID.
| `client_id`     |  String | Optional | The client (application) ID of an App Registration in the tenant
| `client_secret` |  String | Optional | A client secret that was generated for the App Registration.
| `environment`   |  String | Optional | The Azure cloud where your resources exist - `AzureCloud` (default), `AzureChinaCloud`, or `AzureUSGovernment`. 

All arguments are optional, and a `azure` credential with no arguments will behave the same as the [default credential](#default-credential).  


## Attributes (Read-Only)

| Attribute       | Type    |  Description
|-----------------|---------|-----------------
| `env`           | Map     | A map of the resolved credential-related environment variables (`AZURE_TENANT_ID`, `AZURE_CLIENT_SECRET`, `AZURE_CLIENT_ID`, `AZURE_ENVIRONMENT`) 

## Default Credential
The `azure` credential type includes an implicit, default credential (`credential.azure.default`) that will be configured using the Azure environment variables:

```hcl
credential "azure" "default" {
  tenant_id       = env("AZURE_TENANT_ID")
  client_secret   = env("AZURE_CLIENT_SECRET")
  client_id       = env("AZURE_CLIENT_ID")
  environment     = env("AZURE_ENVIRONMENT")
}
```