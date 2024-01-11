---
title:  jira
sidebar_label: jira
---

# jira

The `jira` credential can be used to access Jira resources.

```hcl
credential "jira" "jira_creds" {
   api_token = "ATATT3S........................."
   base_url  = "https://test.atlassian.net/"
   username  = "test@abbb.com"
}
```

## Arguments

| Name            | Type    | Required?| Description
|-----------------|---------|----------|-------------------
| `api_token`     |  String | Optional | API access token  
| `base_url`      |  String | Optional | The Base Url of your Jira API instance 
| `username`      |  String | Optional | The user name to access the Jira cloud instance

All arguments are optional, and a `jira` credential with no arguments will behave the same as the [default credential](#default-credential).  

## Default Credential
The `jira` credential type includes an implicit, default credential (`credential.jira.default`) that will be configured using the environment variables `JIRA_API_TOKEN`, `JIRA_URL`, and `JIRA_USER`.

```hcl
credential "jira" "default" {
   api_token = env("JIRA_API_TOKEN")
   base_url  = env("JIRA_URL")
   username  = env("JIRA_USER")
}
```