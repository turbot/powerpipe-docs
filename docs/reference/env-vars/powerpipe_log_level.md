---
title: POWERPIPE_LOG_LEVEL
---
# POWERPIPE_LOG_LEVEL
Sets the output logging level.  Standard log levels are supported (`DEBUG`, `INFO`, `WARN`, `ERROR`). Logs are written to STDERR.  By default, logging is off.

Logs are written to STDERR.

## Usage 

Turn on info level logging:
```bash
export POWERPIPE_LOG_LEVEL=INFO
```

Turn off logging:

```bash
unset POWERPIPE_LOG_LEVEL
```

Redirect logs to a file:
```bash
POWERPIPE_LOG_LEVEL=DEBUG powerpipe pipeline run my_pipeline 2> mylogs.txt
```