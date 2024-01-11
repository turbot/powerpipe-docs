---
title:  gcp
sidebar_label: gcp
---

# gcp

The `gcp` credential can be used to access Google Cloud Platform resources.

```hcl
credential "gcp" "gcp_def" {
  credentials = "~/.config/gcloud/my_credentials.json"
}
```


## Arguments

| Name            | Type    | Required?| Description
|-----------------|---------|----------|-------------------
| `credentials`   |  String | Optional | Either the path to a JSON credential file that contains Google application credentials or the contents of a service account key file in JSON format.  
| `ttl`           |  Number | Optional |  The time, in seconds, to cache the credentials.  By default, the GP credential will be cached for 5 minutes.


All arguments are optional, and a `gcp` credential with no arguments will behave the same as the [GCP default credential](#default-credential). 



## Attributes (Read-Only)

| Attribute       | Type    |  Description
|-----------------|---------|-----------------
| `access_token`  |  String | An OAuth access token to use to authenticate to GCP. 


## Default Credential
The GCP credential type includes an implicit, default credential (`credential.gcp.default`) that will be configured using the same mechanism as the GCloud CLI (environment variables, config files, etc); the effective credentials of this default are the same as if you run the `gcloud` command.  Credentials will be loaded from:
- The path specified in the `GOOGLE_APPLICATION_CREDENTIALS` environment variable, if set; otherwise
- The standard location (`~/.config/gcloud/application_default_credentials.json`)

## Examples

### Static Credentials from Credential File
```hcl
credential "gcp" "gcp_def" {
  credentials = "~/.config/gcloud/my_credentials.json"
}
```

### Static Credentials defined inline
```hcl
credential "gcp_inline" {
  #project = "my_project"
  credentials = <<EOC
  {
  "client_id": "blah-blah.apps.googleusercontent.com",
  "client_secret": "d-blah",
  "quota_project_id": "dev-aaa",
  "refresh_token": "1//blah-blah",
  "type": "authorized_user"
  } 
  EOC
}
```


`

### Using GCP Credentials in Container Step

```hcl
pipeline "gcp_test" {
    param "cred" {
    type    = string
    default = "default"
  }

  step "container" "get_instances" {
    image = "gcr.io/google.com/cloudsdktool/google-cloud-cli"
    cmd   = ["gcloud", "compute", "instances", "list"]
    env = {
      CLOUDSDK_CORE_PROJECT      = "my-project"
      CLOUDSDK_AUTH_ACCESS_TOKEN = credentials.gcp[param.cred].access_token
    }
  }
}
```


### Using GCP Credentials in HTTP Step

```hcl
pipeline "gcp_test" {
  param "cred" {
    type    = string
    default = "default"
  }

  step "http" "get_instances" {
    url = "https://compute.googleapis.com/compute/v1/projects/my-project/aggregated/instances"
    
    request_headers = {
      Content-Type = "application/json; charset=utf-8"
      Authorization = "Bearer ${credentials.gcp[param.cred].access_token}"
    }
  }
}
```