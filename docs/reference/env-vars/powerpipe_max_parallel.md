---
title: POWERPIPE_MAX_PARALLEL
---

# POWERPIPE_MAX_PARALLEL
Set the maximum number of database connections to open.  Powerpipe will attempt to run up to this many queries in parallel.  This is essentially a connection pool size - Powerpipe will open a database connection for each parallel instance.  The default is 10.

## Usage 
```bash
export POWERPIPE_MAX_PARALLEL=3
```