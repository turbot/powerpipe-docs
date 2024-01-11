---
title:  mastodon
sidebar_label: mastodon
---

# mastodon

The `mastodon` credential can be used to access Mastodon resources.

```hcl
credential "mastodon" "mastodon_creds" {
  server = "https://myserver.social"
  access_token = "FK1_fake-token"
}
```

## Arguments

| Name            | Type    | Required?| Description
|-----------------|---------|----------|--------------------------------------
| `server`        |  String | Optional | The federated server your account lives
| `access_token`  |  String | Optional | Mastodon access token

All arguments are optional, and a `mastodon` credential with no arguments will behave the same as the [default credential](#default-credential).  
