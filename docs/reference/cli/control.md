---
title: powerpipe control
sidebar_label: powerpipe control
---


# powerpipe control

List, view, and run Powerpipe controls.

## Usage
```bash
powerpipe control list [args]
powerpipe control show control_name [args]
powerpipe control run control_name [args]
```


## Sub-Commands

| Command | Description
|-|-
| [list](#powerpipe-control-list) | List controls from the current mod and its direct dependents.
| [run](#powerpipe-control-run)  | Run a control from the current mod or its direct dependents.
| [show](#powerpipe-control-show) | Show details of a control from the current mod or its direct dependents.



----
## powerpipe control list
List controls from the current mod and its direct dependents.

### Examples


List controls:
```bash
powerpipe control list
```


List all controls in `JSON` format:
```bash
powerpipe control list --output json
```

List controls using settings from a workspace:
```bash
powerpipe control list --workspace my_workspace
```


---

## powerpipe control show
Show details of a control from the current mod or its direct dependents.


### Examples

Show details of a single control in the current mod:
```bash
powerpipe control show cis_v200_2_1_1
# or
powerpipe control show control.cis_v200_2_1_1
```


Show details of a single control in a direct dependency mod:
```bash
powerpipe control show aws_compliance.control.cis_v200_2_1_1
```

Show details of a control in `JSON` format:
```bash
powerpipe control show cis_v200_2_1_1 --output json
```


Show details of a control using settings from a workspace:
```bash
powerpipe control show cis_v200_2_1_1 -workspace my_workspace
```
---

## powerpipe control run
Run a control from the current mod or its direct dependents.

### Arguments

| Flag | Description
|-|-
|  `--database`         |  Sets the [database that Powerpipe will connect to](/docs/run#selecting-a-database). This defaults to the local Steampipe database, but can be any PostgreSQL, MySQL, DuckDB, or SQLite database. See [POWERPIPE_DATABASE](/docs/reference/env-vars/powerpipe_database) for details.
|  `--export string`              | Export control output to a file. You may export multiple output formats for a single control run by entering multiple `--export` arguments. If a file path is specified as an argument, its type will be inferred by the suffix. Supported export formats are `asff`, `csv`, `html`, `json`, `md`,`nunit3`, `pps` (snapshot)
|  `--header string`              | Specify whether to include column headers in csv output/export (default `true`).
|  `--input`                      | Enable/Disable interactive prompts for missing variables. To disable prompts and fail on missing variables, use  `--input=false`. This is useful when running from scripts. (default `true`)
|  `--mod-install`                | Specify whether to install mod dependencies before running the control (default `true`)
|  `--output string`              | Select the console output format. Defaults to text. Possible values are `brief`, `csv`, `html`, `json`, `md`, `pps` (snapshot), `pretty`, `plain`, `none`
|  `--pipes-host`                 | Sets the Turbot Pipes host used when connecting to Turbot Pipes workspaces. See  [PIPES_HOST](/docs/reference/env-vars/pipes_host) for details.
|  `--pipes-token`                | Sets the Turbot Pipes authentication token used when connecting to Turbot Pipes workspaces. See  [PIPES_TOKEN](/docs/reference/env-vars/pipes_token) for details.
|  `--progress`                   | Enable or disable progress information. By default, progress information is shown - set  `--progress=false` to hide the progress bar.
|  `--query-timeout int`          | The query timeout, in seconds. The default is `300`.
|  `--search-path strings`        | Set a comma-separated list of connections to use as a custom search path for the control run.
|  `--search-path-prefix strings` | Set a comma-separated list of connections to use as a prefix to the current search path for the control run.
|  `--separator string`           | A single character to use as a separator string for csv output (defaults to `,`)
|  `--share`                      | Create snapshot in Turbot Pipes with `anyone_with_link` visibility.
|  `--snapshot`                   | Create snapshot in Turbot Pipes with the default (`workspace`) visibility.
|  `--snapshot-location string`   |	The location to write snapshots - either a local file path or a Turbot Pipes workspace
|  `--snapshot-tag string=string` | Specify tags to set on the snapshot. Multiple `--snapshot-tag `arguments may be passed.
|  `--snapshot-title string=string` | The title to give a snapshot when uploading to Turbot Pipes.
|  `--tag string=string`          | Filter the list of controls to run by one or more tag values. Multiple `--tag `arguments may be passed. Discrete keys are and'ed and duplicate keys are or'ed. For example, `steampipe check all --tag pci=true --tag service=ec2 --tag service=iam` will run only controls with a `service` tag equal to either `ec2` or `iam` that also are tagged with `pci=true`.
|  `--timing`                     | Turn on the query timer.
| `--var string=string`           | Specify the value of a variable.  Multiple `--var` arguments may be passed. 
| `--var-file strings`            | Specify a `.ppvar` file containing variable values.


### Output Formats

`powerpipe control run` supports all of the [benchamrk output formats](/docs/reference/cli/benchmark#output-formats).



### Examples

Run a control 
```bash
powerpipe control run cis_v200_2_1_1
```

Run a control against a pipes workspace:
```bash
powerpipe control run cis_v200_2_1_1 --workspace acme/anvils
```

Run a control against a specific database:
```bash
powerpipe control run cis_v200_2_1_1 --database  postgres://myusername:passworrd@mydbserver.mydomain.com:9193/steampipe
```


Run a control and upload a snapshot with `workspace` visibility in your user workspace.
```bash
powerpipe control run cis_v200_2_1_1 --snapshot
```


Run a control and upload a snapshot with `anyone_with_link` visibility in your user workspace.
```bash
powerpipe control run cis_v200_2_1_1 --share 
```


Run a control and upload a snapshot with `anyone_with_link` visibility to specific workspace.
```bash
powerpipe control run cis_v200_2_1_1 --share  --snapshot-location vandelay-industries/latex
```

Run a control, upload a snapshot with `workspace` visibility in your user workspace, and tag the snapshot:
```bash
powerpipe control run cis_v200_2_1_1 --snapshot --snapshot-tag env=local 
```