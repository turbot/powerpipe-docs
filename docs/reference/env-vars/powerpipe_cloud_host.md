---
title: POWERPIPE_CLOUD_HOST
sidebar_label: POWERPIPE_CLOUD_HOST
---

# POWERPIPE_CLOUD_HOST

Sets the Turbot Pipes host used when connecting to Turbot Pipes workspaces.  The default is `pipes.turbot.com` -- you only need to set this if you are connecting to a remote Turbot Pipes database that is NOT hosted in `pipes.turbot.com`, such as an enterprise tenant instance. Your `POWERPIPE_CLOUD_TOKEN` must be valid for the `POWERPIPE_CLOUD_HOST`.

Alternatively, you can set the cloud host in the [`PIPES_HOST` environment variable](/docs/reference/env-vars/pipes_host). Note that `PIPES_HOST` has lower precedence than `POWERPIPE_CLOUD_HOST` - if both are set then `POWERPIPE_CLOUD_HOST` will be used.

## Usage 
Default to use workspaces in `vandelay.pipes.turbot.com`:

```bash
export POWERPIPE_CLOUD_HOST=vandelay.pipes.turbot.com
export POWERPIPE_CLOUD_TOKEN=tpt_c6f5tmpe4mv9appio5rg_3jz0a8fakekeyf8ng72qr646
```