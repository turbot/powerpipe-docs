---
title: FLOWPIPE_LISTEN
sidebar_label: FLOWPIPE_LISTEN
---

# FLOWPIPE_LISTEN
Specifies the IP addresses on which `flowpipe server` will listen for connections from clients. Currently supported values are `local` (localhost only) or `network` (all IP addresses). The default is `network`.  This value can only be set when the server is started.

## Usage 

When `flowpipe server` runs, listen only on localhost:

```bash
export FLOWPIPE_LISTEN=local
```

When `flowpipe server` runs, listen on all IP addresses:

```bash
export FLOWPIPE_LISTEN=local
# or
unset FLOWPIPE_LISTEN
```