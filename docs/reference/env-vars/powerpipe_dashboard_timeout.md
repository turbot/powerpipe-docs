---
title: POWERPIPE_DASHBOARD_TIMEOUT
---

# POWERPIPE_DASHBOARD_TIMEOUT
Set the dashboard execution timeout, in seconds.  If the dashboard has not finished executing and the timeout is reached, any running and pending queries will be canceled and the dashboard will be returned with partial results.  Queries that were canceled will display a timeout error.

The default is `0` (no timeout).


## Usage 
Set the dashboard execution timeout to 5 minutes

```bash
export POWERPIPE_DASHBOARD_TIMEOUT=300
```