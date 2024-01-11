---
title: FLOWPIPE_HOST
sidebar_label: FLOWPIPE_HOST
---

# FLOWPIPE_HOST
Sets the Flowpipe API host to connect to. By default, Flowpipe commands run in a [client-only context](/docs/run/index#operating-modes) using the mod from the current working directory or the `--mod-location` if specified.  When `FLOWPIPE_HOST` is set (or `--host` is passed on the command line or in a workspace) Flowpipe will instead run the command against the specified server instance.

You may specify the full host and port (e.g. `https://flowpipe.my-org.com:7103`), or use the keyword `local` to connect to the local server instance (`local` is a shortcut for `https://localhost:7103`).



## Usage 
Run Flowpipe commands against the local Flowpipe server instead of the current directory:
```bash
export FLOWPIPE_HOST=local
```

Run Flowpipe commands against a remote server instead of the current directory:
```bash
export FLOWPIPE_HOST=https://flowpipe.my-org.com:7103
```