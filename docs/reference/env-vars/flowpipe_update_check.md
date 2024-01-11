---
title: FLOWPIPE_UPDATE_CHECK
sidebar_label: FLOWPIPE_UPDATE_CHECK
---

# FLOWPIPE_UPDATE_CHECK
Automatic update checking informs you when a new Flowpipe version is available. The `FLOWPIPE_UPDATE_CHECK` environment variable allows you to enable or disable automatic update checking.  Update checking is enabled by default.  Set to `false` to disable update checking.

## Usage 
Disable update check:
```bash
export FLOWPIPE_UPDATE_CHECK=false 
```

Enable update check:
```bash
export FLOWPIPE_UPDATE_CHECK=true
```
or 
```bash
unset FLOWPIPE_UPDATE_CHECK
```