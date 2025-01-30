---
title: POWERPIPE_TELEMETRY
---

# POWERPIPE_TELEMETRY

By default, Powerpipe collects usage information to help assess features, usage patterns, and bugs.  This information helps us improve and optimize the Powerpipe experience.  We do not collect any sensitive information such as secrets, environment variables, or file contents.  We do not share your data with anyone.  Current options are:
- `none`: do not collect or send any telemetry data.
- `info`: send basic information such as which mods are used, and how and when Powerpipe is started and stopped. 

## Usage 

Disable telemetry data:

```bash
export POWERPIPE_TELEMETRY=none
```

Enable telemetry data at `info` level (this is the default)

```bash
export POWERPIPE_TELEMETRY=info
```