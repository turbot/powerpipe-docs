---
title: PIPES_HOST
---

# PIPES_HOST
Sets the Turbot Pipes host used when connecting to Turbot Pipes workspaces.  The default is `pipes.turbot.com` -- you only need to set this if you are connecting to a remote Turbot Pipes database that is NOT hosted in `pipes.turbot.com`, such as an enterprise tenant instance.  Your `PIPES_TOKEN` must be valid for the `PIPES_HOST`.


## Usage 
Default to use workspaces in `vandelay.pipes.turbot.com`:

```bash
export PIPES_HOST=vandelay.pipes.turbot.com
```