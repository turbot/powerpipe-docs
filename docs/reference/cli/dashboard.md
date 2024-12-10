---
title: powerpipe dashboard
---


# powerpipe dashboard

List, view, and run Powerpipe dashboards in batch mode.  To run dashboards interactively, see [powerpipe server](/docs/reference/cli/server).

## Usage

```bash
powerpipe dashboard list [args]
powerpipe dashboard show dashboard_name [args]
powerpipe dashboard run dashboard_name [args]
```

## Sub-Commands

| Command | Description
|-|-
| [list](#powerpipe-dashboard-list) | List dashboards from the current mod and its direct dependents.
| [run](#powerpipe-dashboard-run)  | Run a dashboard from the current mod or its direct dependents.
| [show](#powerpipe-dashboard-show) | Show details of a dashboard from the current mod or its direct dependents.

----
## powerpipe dashboard list
List dashboards from the current mod and its direct dependents.

### Examples


List dashboards:
```bash
powerpipe dashboard list
```

List all dashboards in `JSON` format:
```bash
powerpipe dashboard list --output json
```

List dashboards using settings from a workspace:
```bash
powerpipe dashboard list --workspace my_workspace
```


---

## powerpipe dashboard show
Show details of a dashboard from the current mod or its direct dependents.

### Examples

Show details of a single dashboard in the current mod:
```bash
powerpipe dashboard show account_report
# or
powerpipe dashboard show dashboard.account_report
```

Show details of a single dashboard in a direct dependency mod:
```bash
powerpipe dashboard show aws_insights.dashboard.account_report
```

Show details of a dashboard in `JSON` format:
```bash
powerpipe dashboard show account_report --output json
```


Show details of a dashboard using settings from a workspace:
```bash
powerpipe dashboard show account_report -workspace my_workspace
```

---


## powerpipe dashboard run
Run a dashboard from the current mod or its direct dependents.

### Arguments

| Flag | Description
|-|-
| `--arg string=string`           | Specify the value for a dashboard input. Multiple `--arg` arguments may be passed. 
| `--dashboard-timeout int`       | Set the dashboard execution timeout, in seconds. The default is `0` (no timeout).
|  `--database`         | ***DEPRECATED - See [Setting the Database](/docs/build/mod-database) for the new syntax.***  Sets the [database that Powerpipe will connect to](/docs/run#selecting-a-database). This defaults to the local Steampipe database, but can be any PostgreSQL, MySQL, DuckDB, or SQLite database.
|  `--export string`              | Export dashboard output to a file. You may export multiple output formats for a single dashboard run by entering multiple `--export` arguments. If a file path is specified as an argument, its type will be inferred by the suffix. Supported export formats are `none`, `pps` (`snapshot`)
|  `--input`                      | Enable/Disable interactive prompts for missing variables. To disable prompts and fail on missing variables, use  `--input=false`. This is useful when running from scripts. (default `true`)
|  `--max-parallel int`           | Set the maximum number of database connections to open. When running dashboards, Powerpipe will attempt to run up to this many dashboards in parallel. See the `POWERPIPE_MAX_PARALLEL` environment variable documentation for details. (default `10`)
|  `--mod-install`                | Specify whether to install mod dependencies before running the dashboard (default `true`)
|  `--output string`              | Select the console output format. Defaults to text. Possible values are `none`, `pps` (`snapshot`)
|  `--pipes-host`                 | Sets the Turbot Pipes host used when connecting to Turbot Pipes workspaces. See  [PIPES_HOST](/docs/reference/env-vars/pipes_host) for details.
|  `--pipes-token`                | Sets the Turbot Pipes authentication token used when connecting to Turbot Pipes workspaces. See  [PIPES_TOKEN](/docs/reference/env-vars/pipes_token) for details.
|  `--progress`                   | Enable or disable progress information. By default, progress information is shown - set  `--progress=false` to hide the progress bar.
|  `--query-timeout int`          | The query timeout, in seconds. The default is `300`.
|  `--search-path strings`        | Set a comma-separated list of connections to use as a custom search path for the dashboard run.
|  `--search-path-prefix strings` | Set a comma-separated list of connections to use as a prefix to the current search path for the dashboard run.
|  `--share`                      | Create snapshot in Turbot Pipes with `anyone_with_link` visibility.
|  `--snapshot`                   | Create snapshot in Turbot Pipes with the default (`workspace`) visibility.
|  `--snapshot-location string`   |	The location to write snapshots - either a local file path or a Turbot Pipes workspace
|  `--snapshot-tag string=string` | Specify tags to set on the snapshot. Multiple `--snapshot-tag `arguments may be passed.
|  `--snapshot-title string=string` | The title to give a snapshot when uploading to Turbot Pipes.
| `--var string=string`           | Specify the value of a variable.  Multiple `--var` arguments may be passed. 
| `--var-file strings`            | Specify a `.ppvar` file containing variable values.


### Examples

Run a dashboard and output the snapshot data:
```bash
powerpipe dashboard run account_report
```

Run a dashboard and output the snapshot data to a file:
```bash
powerpipe dashboard run account_report > mysnap.pps
# or
powerpipe dashboard run account_report --export mysnap.pps
```


Run a dashboard and upload a snapshot with `workspace` visibility in your user workspace.
```bash
powerpipe dashboard run account_report --snapshot  
```

Run a dashboard and upload a snapshot with `anyone_with_link` visibility in your user workspace.
```bash
powerpipe dashboard run --share account_report 
```

Run a dashboard and save a [snapshot](/docs/run/snapshots/batch-snapshots), specifying inputs:

```bash
powerpipe dashboard run aws_insights.dashboard.aws_vpc_detail \
  --snapshot \
  --dashboard-input vpc_id=vpc-9d7ae1e7
```


Run a dashboard and upload a snapshot with `anyone_with_link` visibility to a specific workspace.
```bash
powerpipe dashboard run account_report --share  --snapshot-location vandelay-industries/latex 
```

Run a dashboard, upload a snapshot with `workspace` visibility in your user workspace, and tag the snapshot:
```bash
powerpipe dashboard run account_report --snapshot --snapshot-tag env=local 
```

Run a dashboard against a pipes workspace:
```bash
powerpipe dashboard run account_report --workspace acme/anvils
```