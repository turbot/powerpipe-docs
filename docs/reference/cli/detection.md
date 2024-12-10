---
title: powerpipe detection
---


# powerpipe detection

List, view, and run powerpipe detections.

## Usage

```bash
powerpipe detection list [args]
powerpipe detection show [args]
powerpipe detection run [args]
```

## Sub-Commands

| Command | Description
|-|-
| [list](#powerpipe-detection-list) | List detections from the current mod and its direct dependents **[[true about dependents? or will be true?]]**.
| [run](#powerpipe-detection-run)  | Run a detection from the current mod or its direct dependents.
| [show](#powerpipe-detection) | Show details of a detection from the current mod or its direct dependents.


## powerpipe detection list
List detections from the current mod and its direct dependents.


### Examples

List detections:
```bash
powerpipe detection list
```

List all detections in `JSON` format:
```bash
powerpipe detection list --output json
```

List detections using settings from a workspace:
```bash
powerpipe detection list --workspace my_workspace
```
## powerpipe detection show
Show details of a detection from the current mod or its direct dependents.

### Examples

Show details of a single detection in the current mod:
```bash
powerpipe detection show audit_logs_detect_failed_workflow_actions
# or
powerpipe detection show detection.audit_logs_detect_failed_workflow_actions
```

Show details of a single detection in a direct dependency mod: **[[not yet?]]**
```bash
powerpipe detection show xxx.detection.audit_logs_detect_failed_workflow_actions
```

Show details of a detection in `JSON` format:
```bash
powerpipe detection show audit_logs_detect_failed_workflow_actions --output json
```

Show details of a detection using settings from a workspace:
```bash
powerpipe detection show audit_logs_detect_failed_workflow_actionst --workspace my_workspace
```

## powerpipe detection run
Run a detection from the current mod or its direct dependents.

### Arguments

Usage:
  powerpipe detection run [flags] [detection]

>[!NOTE]
>strikethroughs == maybe not for lw7?
>
> snapshots:  you can save/load locally but not to pipes yet until tailpipe is there, right?
>
> other thoughts on what to keep/toss/modify?

| Flag                     | Description |
|-|-|
| `--arg stringArray`      | Specify the value of a detection argument. |
| `--database string`      | Turbot Pipes workspace database. **DEPRECATED**: See [documentation](https://powerpipe.io/docs/run#selecting-a-database) for the new syntax. |
| `--detection-timeout int`| Set the detection execution timeout. |
| `--export strings`       | Export output to file. Supported format: `pps` (snapshot).|
| `-h, --help`             | Help for detection. |
| `--input`                | Enable interactive prompts. (default: `true`) |
| `--max-parallel int`     | The maximum number of concurrent database connections to open. (default: `10`) |
| `--mod-install`          | Specify whether to install mod dependencies before running the detection. (default: `true`) |
| `--mod-location string`  | Sets the workspace working directory. If not specified, the workspace directory will be set to the current working directory. (default: `/home/jon/tailpipe-mod-github-detections`) |
| `--output output`        | Output format: `snapshot`, `pps`, `none`. (default: `pps`) |
| ~~`--pipes-host string`~~    | ~~Turbot Pipes host. (default: `pipes.turbot.com`)~~ |
| ~~`--pipes-token string`~~   | ~~Turbot Pipes authentication token.~~ |
| `--progress`             | Display detection execution progress when a detection name argument is passed. (default: `true`) |
| `--pull pull`            | Update strategy: `full`, `latest`, `development`, `minimal`. (default: `minimal`) |
| `--query-timeout int`    | The query timeout in seconds. (default: `300`) |
| ~~`--share`~~                | ~~Create snapshot in Turbot Pipes with `anyone_with_link` visibility.~~ |
| ~~`--snapshot`~~             | ~~Create snapshot in Turbot Pipes with the default (workspace) visibility.~~ |
| ~~`--snapshot-location string`~~ | ~~The location to write snapshots: either a local file path or a Turbot Pipes workspace.~~ |
| ~~`--snapshot-tag stringArray`~~ | ~~Specify tags to set on the snapshot.~~ |
| ~~`--snapshot-title string`~~| ~~The title to give a snapshot.~~ |
| `--var stringArray`      | Specify the value of a variable. |
| `--var-file strings`     | Specify a `.ppvar` file containing variable values. |



### Examples

Run a detection and output the snapshot data:
```bash
powerpipe detection run audit_logs_detect_failed_workflow_actions
```

Run a detection and output the snapshot data to a file:
```bash
powerpipe detection run audit_logs_detect_failed_workflow_actions > mysnap.pps
# or
powerpipe detection run audit_logs_detect_failed_workflow_actions --export mysnap.pps
```

<!--

Run a detection and upload a snapshot with `workspace` visibility in your user workspace.
```bash
powerpipe detection run audit_logs_detect_failed_workflow_actions --snapshot  
```

Run a detection and upload a snapshot with `anyone_with_link` visibility in your user workspace.
```bash
powerpipe detection run --share audit_logs_detect_failed_workflow_actions 
```

Run a detection and upload a snapshot with `anyone_with_link` visibility to a specific workspace.
```bash
powerpipe detection run audit_logs_detect_failed_workflow_actions --share  --snapshot-location vandelay-industries/latex 
```

Run a detection, upload a snapshot with `workspace` visibility in your user workspace, and tag the snapshot:
```bash
powerpipe detection run audit_logs_detect_failed_workflow_actions --snapshot --snapshot-tag env=local 
```

Run a detection against a pipes workspace:
```bash
powerpipe detection run audit_logs_detect_failed_workflow_actions --workspace acme/anvils
```
-->