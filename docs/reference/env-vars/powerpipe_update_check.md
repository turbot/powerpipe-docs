---
title: POWERPIPE_UPDATE_CHECK
---

# POWERPIPE_UPDATE_CHECK
Automatic update checking informs you when a new Powerpipe version is available. The `POWERPIPE_UPDATE_CHECK` environment variable allows you to enable or disable automatic update checking.  Update checking is enabled by default.  Set to `false` to disable update checking.

## Usage 
Disable update check:
```bash
export POWERPIPE_UPDATE_CHECK=false 
```

Enable update check:
```bash
export POWERPIPE_UPDATE_CHECK=true
```
or 
```bash
unset POWERPIPE_UPDATE_CHECK
```