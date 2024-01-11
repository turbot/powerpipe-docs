---
title: FLOWPIPE_LOG_LEVEL
sidebar_label: FLOWPIPE_LOG_LEVEL
---
# FLOWPIPE_LOG_LEVEL
Sets the output logging level.  Standard log levels are supported (`DEBUG`, `INFO`, `WARN`, `ERROR`). Logs are written to STDERR.  By default, logging is off.

Logs are written to STDERR.

## Usage 

Turn on info level logging:
```bash
export FLOWPIPE_LOG_LEVEL=INFO
```

Turn off logging:

```bash
unset FLOWPIPE_LOG_LEVEL
```

Redirect logs to a file:
```bash
FLOWPIPE_LOG_LEVEL=DEBUG flowpipe pipeline run my_pipeline 2> mylogs.txt
```