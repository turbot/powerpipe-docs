---
title:  openai
sidebar_label: openai
---

# openai

The `openai` credential can be used to access OpenAI resources.

```hcl
credential "openai" "my_openai" {
  api_key = "sk-jwgthNa..."
}
```

## Arguments

| Name            | Type    | Required?| Description
|-----------------|---------|----------|-------------------
| `api_key`       |  String | Optional | API key  


All arguments are optional, and an `openai` credential with no arguments will behave the same as the [default credential](#default-credential).  

## Default Credential
The `openai` credential type includes an implicit, default credential (`credential.openai.default`) that will be configured to set the `api_key` to the `OPENAI_API_KEY` environment variable.

```hcl
credential "openai" "default" {
  api_key = env("OPENAI_API_KEY")
}
```
