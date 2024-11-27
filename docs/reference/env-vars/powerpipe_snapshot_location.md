---
title: POWERPIPE_SNAPSHOT_LOCATION
---


# POWERPIPE_SNAPSHOT_LOCATION
Sets the location to write [batch snapshots](/docs/run/snapshots/batch-snapshots) - either a local file path or a [Turbot Pipes workspace](https://turbot.com/pipes/docs/workspaces).

By default, Powerpipe will write snapshots to your default Turbot Pipes user workspace.

## Usage 
Set the snapshot location to a local filesystem path:

```bash
export POWERPIPE_SNAPSHOT_LOCATION=~/my-snaps
```


Set the snapshot location to a Turbot Pipes workspace:

```bash
export POWERPIPE_SNAPSHOT_LOCATION=vandelay-industries/latex 
```