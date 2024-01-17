---
title: PIPES_HOST
sidebar_label: PIPES_HOST
---

# PIPES_HOST
Sets the Turbot Pipes host used when connecting to Turbot Pipes workspaces.  The default is `pipes.turbot.com` -- you only need to set this if you are connecting to a remote Turbot Pipes database that is NOT hosted in `pipes.turbot.com`, such as an enterprise tenant instance.  Your `PIPES_TOKEN` must be valid for the `PIPES_HOST`.

Alternatively, you can set the cloud host in the [`POWERPIPE_CLOUD_HOST` environment variable](/docs/reference/env-vars/powerpipe_cloud_host).  Note that `PIPES_HOST` has lower precedence than `POWERPIPE_CLOUD_HOST` - if both are set then `POWERPIPE_CLOUD_HOST` will be used.

## Usage 
Default to use workspaces in `vandelay.pipes.turbot.com`:

```bash
export PIPES_HOST=vandelay.pipes.turbot.com
export PIPES_TOKEN=tpt_c6f5tmpe4mv9appio5rg_3jz0a8fakekeyf8ng72qr646
```