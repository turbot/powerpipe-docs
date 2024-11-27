---
title: POWERPIPE_LISTEN
---

# POWERPIPE_LISTEN
Specifies the IP addresses on which `powerpipe server` will listen for connections from clients. Currently supported values are `local` (localhost only) or `network` (all IP addresses). The default is `network`.  This value can only be set when the server is started.

## Usage 

When `powerpipe server` runs, listen only on localhost:

```bash
export POWERPIPE_LISTEN=local
```

When `powerpipe server` runs, listen on all IP addresses:

```bash
export POWERPIPE_LISTEN=local
# or
unset POWERPIPE_LISTEN
```