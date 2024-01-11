---
title:  aws
sidebar_label: aws
---

# aws

The `aws` credential can be used to access Amazon Web Services resources.

```hcl
credential "aws" "my_creds" {
  profile = "aws-account-01"
}
```

## Arguments

| Name            | Type    | Required?| Description
|-----------------|---------|----------|-------------------
| `profile`       |  String | Optional | The [AWS Profile](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html) to use to obtain credentials. 
| `access_key`    |  String | Optional | A static AWS access key to use to authenticate.
| `secret_key`    |  String | Optional | A static AWS secret key to use to authenticate.
| `session_token` |  String | Optional |  A static AWS session token to use to authenticate.  This is only used if you specify `access_key` and `secret_key`.
| `ttl`           |  Number | Optional |  The time, in seconds, to cache the credentials.  By default, the AWS credential will be cached for 5 minutes.


All arguments are optional, and an `aws` credential with no arguments will behave the same as the [AWS default credential](#default-credential).  You may instead specify exactly one of:
  - `profile`
  - `access_key` and `secret_key` (and optionally `session_token`)


## Attributes (Read-Only)

| Attribute       | Type    |  Description
|-----------------|---------|-----------------
| `access_key`    |  String | The AWS access key to use to authenticate. If you specified the `access_key` arg, then this is the argument value verbatim.  If you have specified `profile`, then this is the resolved temporary access key for that profile.
| `secret_key`    |  String | The AWS secret key to use to authenticate.  If you specified the `secret_key` arg, then this is the argument value verbatim.  If you have specified `profile`, then this is the resolved temporary secret key for that profile.
| `session_token` |  String | The AWS session token to use to authenticate.  If you specified the `session_token` arg, then this is the argument value verbatim.  If you have specified `profile`, then this is the resolved temporary session token for that profile.
| `env`           | Map     | A map of the resolved [credential-related environment variables](https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/guide_credentials_environment.html) (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_SESSION_TOKEN`, `AWS_PROFILE`) 

## Default Credential
The AWS credential type includes an implicit, default credential (`credential.aws.default`) that will be configured using the same mechanism as the AWS CLI (AWS environment variables, default profile, etc); the effective credentials of this default are the same as if you run the `aws` command.


## Examples

### Static Credentials
```hcl
credential "aws" "aws_static" {
  access_key = "ASIAQGDFAKEKGUI5MCEU"
  secret_key = "QhLNLGM5MBkXiZm2k2tfake+TduEaCkCdpCSLl6U"
}
```

### AWS Profile
```hcl
credential "aws" "aws_profile" {
  profile = "awx_prod_01"
}
```

### Using AWS Credentials in Container Step

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
