---
title: Managing Credentials
sidebar_label: Credentials
---

# Managing Credentials

Flowpipe **Credentials** provide a mechanism for defining and sharing secrets in your Flowpipe environment.


## Defining / Declaring  Credentials
`credential` is a top-level resource in a config file (.fpc). Credentials are defined in configuration files (`.fpc`), NOT mod files (`.fp`).  Like `workspace`, a `credential` can be defined in any .fpc file in the config directory (`~/.flowpipe/config/*.fpc`) or in the root of the `mod-location` (usually `$PWD/*.fpc`). 

While it is *possible* to add credentials in `.fpc` in a mod repo, you should usually avoid doing so:
  - They often contain secrets
  - Different users will have different secrets - you should not share credentials

Credentials are defined in a `credential` block.  They have a type label and a name.

```hcl
credential "aws" "aws_01" {
  access_key = "ASIAQGDFAKEKGUI5MCEU"
  secret_key = "QhLNLGM5MBkXiZm2k2tfake+TduEaCkCdpCSLl6U"
}
```

There is a type for each service:  `aws`, `gcp`, `azure`, `github`, `slack`, etc...  The arguments and attributes vary by type. 

```hcl
credential "gcp" "gcp_01" {
  credentials    = "~/.config/gcloud/application_default_credentials.json"
}
credential "slack" "slack" {
  token    = "xoxp-asd7f8fd0fake9890"
}
```


You can use the `env()` function to set credentials to the value of an environment variable:
```hcl
credential "slack" "default" {
  token    = env("SLACK_TOKEN")
}
```

### Default credentials
Flowpipe also creates a single **implicit default** credential for each credential type.  The credential is named `default`, eg `credential.aws.default`, `credential.github.default`, etc.  The intent of the default credential is that Flowpipe can "just work" with your existing setup, and is similar to the default connection that Steampipe creates for each plugin.  For example, the AWS default credential will use the standard AWS SDK credential resolution mechanism to create a Flowpipe credential that will use the same credential that the `aws` CLI would use. 

You can override the default by simply creating a credential for that type that is named `default`:

```hcl
credential "slack" "default" {
  token    = "xoxp-mydefaulttoken"
}
```

<!--
You can also turn off the implicit default credentials by passing `--implicit-credentials=false` to the Flowpipe command:

```bash
flowpipe server --implicit-credentials=false
```
-->

----

## Using / Referencing Credentials 

Although credentials are defined in config files (not mod files), Flowpipe makes all the credentials available to mods using the `credential` variable.  This special variable is a map of all credential types by type and name, so you can reference it with standard HCL syntax: `credential.{type}.{name}` eg `credential.aws.aws01` or `credential[{type}][{name}]` eg `credential["aws"["aws01"]`.

You can reference them directly in a mod:
```hcl
pipeline "ex1" {
  step "container" "aws" {
    image = "public.ecr.aws/aws-cli/aws-cli"
    cmd   = [ "s3", "ls" ]
    env   = credential.aws.aws01.env
  } 
}
```

Since they are a map, you can even loop through them:

```hcl
pipeline "ex1" {
  step "container" "aws" {
    for_each = credential.aws
    image    = "public.ecr.aws/aws-cli/aws-cli"
    cmd      = [ "s3", "ls" ]
    env      = credential.aws[each.value].env
  } 
}
```

Or reference them dynamically:
<!--
 - this can be especially useful when [importing](#importing-credentials) Steampipe credentials:
-->

```hcl
pipeline "ex1" {
  step "query" "get_running_instances" {
    sql = <<-EOQ
      select
        instance_id,
        region,
        account_id,
        _ctx ->> 'connection_name' as connection_name,
        instance_state,
        tags
      from
        aws_ec2_instance
      where
        instance_state == 'running'
        and tags ->> 'env' like 'dev%'
    EOQ
  }

  step "container" "stop_instances" {
    for_each = step.query.get_running_instances.rows
    image    = "public.ecr.aws/aws-cli/aws-cli"
    cmd     = [ "ec2", "stop-instances" 
                "--instance-ids", each.value["instance_id"
                "--region", each.value["region"]]
    ]
    env   = credential.aws[each.value.connection_name].env
  } 
}
```

### Passing credentials
Generally, referencing credentials literally by name in a mod is bad practice, especially in library mods because the details are instance-specific - You are making assumptions about what credentials have been set up, including the names of those credentials.  Instead, you should generally pass in the credential name as a pipeline parameter.  By convention, you should set the value of the credential name to `default` so that it will use the [default credentials](#default-credentials) if no credential is passed:

```hcl
pipeline "ex01" {

  param "cred" {
    type = string
    default = "default"
  }

  step "http" "send_msg" {
    url = "https://slack.com/...."
    
    request_headers = {
      Content-Type = "application/json; charset=utf-8"
      Authorization = "Bearer ${credential.slack[param.cred].token}"
    }
  }

  output "results_http" {
    value = step.http.send_msg.response_body
  }

}
```

You can run the pipeline without argument to use your slack default credentials:

```bash
flowpipe pipeline run ex01
```

If you want to use a different credential instead, you can pass it as an argument:

```bash
flowpipe pipeline run ex01 --arg cred=my_other_slack_credential
```


### Dynamic Credentials

Some credentials support dynamic attributes that contain temporary credentials.  Cloud services like AWS and GCP provide many mechanisms for defining credentials (static keys, SSO, assumed roles, profiles,etc) but in all cases can be exchanged for a "standard" format such as an access token or AWS key pair.

For example, you can specify AWS credentials in many formats:
```hcl
credential "aws" "aws_static" {
  access_key = "ASIAQGDFAKEKGUI5MCEU"
  secret_key = "QhLNLGM5MBkXiZm2k2tfake+TduEaCkCdpCSLl6U"
}

credential "aws" "aws_profile" {
  profile = "awx_prod_01"
}

```

For *all* of these cases you can resolve to an access key / secret key / session token, which are available as attributes. Additionally, the `aws` credential provides an `env` attribute which is a map of the [credential-related environment variables](https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/guide_credentials_environment.html) (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_SESSION_TOKEN`) 

When writing a library mod, you should use these dynamic attributes where possible, as it simplifies the code and minimizes the number of credential-related parameters:

```hcl
pipeline "ex1" {
  param "cred" {
    type    = string
    default = "default"
  }

  step "container" "aws" {
    image = "public.ecr.aws/aws-cli/aws-cli"
    cmd   = [ "s3", "ls" ]
    env   = credential.aws[param.cred].env
  } 
}
```

When using dynamic credential attributes, the credentials are resolved as late as possible.  When the *step instance* runs that uses the credential, it is fetched (from the cache if not expired) or generated.  This is important because the dynamic credentials are usually temporary and will expire and become invalid. You can tell Flowpipe how long to cache these temporary credentials with the `ttl` argument.  This specifies the number of seconds to cache the credentials.  Not all credentials support (or require) `ttl`, and for those that do, the default value varies by type.  Note that the cache is in-memory only, and does not persist when Flowpipe stops.

```hcl
credential "gcp" " my_creds" {
  credentials = "~/.config/gcloud/application_default_credentials.json"
  ttl         = 900 # 15 minutes
}
```
