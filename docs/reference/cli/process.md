---
title: flowpipe process
sidebar_label: flowpipe process
---

# flowpipe process

Manage Flowpipe variables in the current mod and its direct dependents.


## Usage
```bash
flowpipe process [command] [flags]
```

## Sub-Commands

| Command | Description
|-|-
| `list` | List processes.
| `tail` | Display events for a processes running in [server mode](/docs/run/index#operating-modes). If the process is still running, Flowpipe will tail the process until it completes.  You may only use `--output json` or `--output yaml` for processes that are stopped.
| `show` | Show details for a single process. 

<!--
Note that `flowpipe pipeline show` supports additional output formats: `sps` (Steampipe snapshot) 
-->

## Flags

| Flag | Applies to | Description
|-|-|-
| `--verbose`   | `tail` | View detailed event information, including step arguments and attributes.


## Examples

List processes:

```bash
flowpipe process list
```

List processes for a server instance:

```bash
flowpipe process list --host local
```

View process information:

```bash
flowpipe process show exec_cl4l9ibjtoj9mk6ol0l0
```

View process events:

```bash
flowpipe process tail exec_cl4l9ibjtoj9mk6ol0l0 --host local
```

View detailed process events:

```bash
flowpipe process tail exec_cl4l9ibjtoj9mk6ol0l0 --host local --verbose
```

List processes in JSON format:

```bash
flowpipe process list --output json
```

<!--
Export Steampipe dashboard snapshot of a process:

```bash
flowpipe process show exec_cl4l9ibjtoj9mk6ol0l0 --output sps > exec_cl4l9ibjtoj9mk6ol0l0.sps
```
-->