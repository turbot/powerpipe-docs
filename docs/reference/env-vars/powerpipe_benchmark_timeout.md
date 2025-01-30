---
title: POWERPIPE_BENCHMARK_TIMEOUT
---

# POWERPIPE_BENCHMARK_TIMEOUT
Set the benchmark execution timeout, in seconds.  If the benchmark has not finished executing and the timeout is reached, any running and pending queries will be canceled and the benchmark will be returned with partial results.  Queries that were canceled will display a timeout error.

The default is `0` (no timeout).


## Usage 
Set the benchmark execution timeout to 15 minutes

```bash
export POWERPIPE_BENCHMARK_TIMEOUT=900
```