---
title:  teams
sidebar_label: teams
---

# teams

The `teams` credential can be used to access Microsoft Teams resources.

```hcl
credential "teams" "my_teams" {
  access_token= "bfc6f1c4fakesdfdxxxx26fake977b2xxxsfsdfakef313c3d389126de0d"
}
```

## Arguments

| Name            | Type    | Required?| Description
|-----------------|---------|----------|-------------------
| `access_token`  |  String | Optional | API token


All arguments are optional, and a `teams` credential with no arguments will behave the same as the [default credential](#default-credential).  

## Default Credential
The `teams` credential type includes an implicit, default credential (`credential.teams.default`) that will be configured to set the `access_token` to the `TEAMS_ACCESS_TOKEN` environment variable.

```hcl
credential "teams" "default" {
  access_token = env("TEAMS_ACCESS_TOKEN")
}
```