---
title:  credential
sidebar_label: credential
---

#  Credential

Flowpipe **Credentials** provide a mechanism for defining and sharing secrets in your Flowpipe environment.

```hcl
credential "aws" "aws_01" {
  profile = "aws-account-01"
}

credential "gcp" "gcp_01" {
  credentials = "~/.config/gcloud/application_default_credentials.json"
}

credential "slack" "slack" {
  token = "xoxp-asd7f8fd0fake9890"
}
```


Credentials are defined using the `credential` block in one or more Flowpipe config files.  Flowpipe will load ALL configuration files (`*.fpc`) from every directory in the [configuration search path](/docs/reference/env-vars/flowpipe_config_path), with decreasing precedence. The set of credentials is the union of all credentials defined in these directories.  

Each credential has a type label and a name. There is a type for each service:  [aws](/docs/reference/config-files/credential/aws), [gcp](/docs/reference/config-files/credential/gcp), [azure](/docs/reference/config-files/credential/azure), [github](/docs/reference/config-files/credential/github), [slack](/docs/reference/config-files/credential/slack), etc.  **The arguments and attributes vary by type.**


To learn more, see **[Managing Credentials →](/docs/run/credentials)**



### Default credentials
Flowpipe also creates a single **implicit default** credential for each credential type.  The credential is named `default`, eg `credential.aws.default`, `credential.github.default`, etc.  The intent of the default credential is that flowpipe can "just work" with your existing setup, and is similar to the default connection that Steampipe creates for each plugin.  For example, the AWS default credential will use the standard AWS SDK credential resolution mechanism to create a Flowpipe credential that will use the same credential that the `aws` CLI would use. 

You can override the default by simply creating a credential for that type that is named `default`:

```hcl
credential "slack" "default" {
  token    = "xoxp-mydefaulttoken"
}
```

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

### Dynamic Credentials

Some credentials support dynamic attributes that contain temporary credentials.  Cloud services like AWS and GCP provide many mechanisms for defining credentials (static keys, SSO, assumed roles, profiles,etc) but in all cases can be exchanged for a "standard" format such as an access token or AWS key pair.

When using dynamic credential attributes, the credentials are resolved as late as possible.  When the *step instance* runs that uses the credential, it is fetched (from the cache if not expired) or generated.  This is important because the dynamic credentials are usually temporary and will expire and become invalid. You can tell Flowpipe how long to cache these temporary credentials with the `ttl` argument.  This specifies the number of seconds to cache the credentials.  Not all credentials support (or require) `ttl`, and for those that do, the default value varies by type.  Note that the cache is in-memory only, and does not persist when Flowpipe stops.

----

## Handling secret data

At this time, Flowpipe does not encrypt the credentials, either in the config files or in the event store.  

Flowpipe will need the secret data in the event store as long as a pipeline is running.  After a pipeline completes, however, Flowpipe will attempt to redact secrets from the event store for the completed pipeline.  The `args`, `attributes`, and `data` for all events related to the pipeline will be scrubbed.

Additionally, Flowpipe will redact secrets from the CLI display and diagnostic logs.

The redaction will use pattern matching to look for known secret names (`AWS_SECRET_KEY`, `password`, `token`, etc) as well as value patterns (regular expressions for JWT tokens, API tokens, AWS keys, etc).  
