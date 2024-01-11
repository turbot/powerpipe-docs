---
title:  gitlab
sidebar_label: gitlab
---

# gitlab

The `gitlab` credential can be used to access GitLab resources.

```hcl
credential "gitlab" "my_gitlab_token" {
  token = "glpat-..."
}
```

## Arguments

| Name            | Type    | Required?| Description
|-----------------|---------|----------|-------------------
| `token`  |  String | Optional | API access token  


All arguments are optional, and a `gitlab` credential with no arguments will behave the same as the [default credential](#default-credential).  

## Default Credential
The `gitlab` credential type includes an implicit, default credential (`credential.gitlab.default`) that will be configured to set the `token` to the `GITLAB_TOKEN` environment variable.

```hcl
credential "gitlab" "default" {
  token = env("GITLAB_TOKEN")
}
```