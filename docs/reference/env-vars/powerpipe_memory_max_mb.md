---
title: POWERPIPE_MEMORY_MAX_MB
---

# POWERPIPE_MEMORY_MAX_MB
Set a soft memory limit for the `powerpipe` process. Powerpipe sets `GOMEMLIMIT` for the `powerpipe` process to the specified value. The Go runtime does not guarantee that the memory usage will not exceed the limit, but rather uses it as a target to optimize garbage collection.

Set the `POWERPIPE_MEMORY_MAX_MB` to `0` to disable the soft memory limit.

## Usage 

Set the memory soft limit to 2GB:
```bash
export POWERPIPE_MEMORY_MAX_MB=2048
```

Disable the memory soft limit:
```bash
export POWERPIPE_MEMORY_MAX_MB=0
```