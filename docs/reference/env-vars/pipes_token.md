---
title: PIPES_TOKEN
sidebar_label: PIPES_TOKEN
---


# PIPES_TOKEN
Sets the [Turbot Pipes authentication token](https://turbot.com/pipes/docs/da-settings#tokens). This is used when connecting to Turbot Pipes workspaces.  

By default, Powerpipe will use the token obtained by running `powerpipe login`, but you may also set this to user-generated [API token](https://turbot.com/pipes/docs/da-settings#tokens).  You can manage your API tokens from the **Settings** page for your user account in Turbot Pipes.


Alternatively, you can set the cloud host in the [`POWERPIPE_CLOUD_TOKEN` environment variable](/docs/reference/env-vars/powerpipe_cloud_token).  
Note that `PIPES_TOKEN` has lower precedence than `POWERPIPE_CLOUD_TOKEN` - if both are set then `POWERPIPE_CLOUD_TOKEN` will be used.

## Usage 
Set your api token:
```bash
export PIPES_TOKEN=tpt_c6f5tmpe4mv9appio5rg_3jz0a8fakekeyf8ng72qr646
```
