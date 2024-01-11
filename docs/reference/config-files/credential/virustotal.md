---
title:  virustotal
sidebar_label: virustotal
---

# virustotal

The `virustotal` credential can be used to access VirusTotal resources.

```hcl
credential "virustotal" "my_ virustotal" {
  api_key = "SG.R7..."
}
```

## Arguments

| Name            | Type    | Required?| Description
|-----------------|---------|----------|-------------------
| `api_key`         |  String | Optional | API key  


All arguments are optional, and a `virustotal` credential with no arguments will behave the same as the [default credential](#default-credential).  

## Default Credential
The `virustotal` credential type includes an implicit, default credential (`credential.virustotal.default`) that will be configured to set the `api_key` to the `VTCLI_APIKEY` environment variable.

```hcl
credential "virustotal" "default" {
  api_key = env("VTCLI_APIKEY")
}
```