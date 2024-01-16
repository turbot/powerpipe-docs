---
title: powerpipe benchmark
sidebar_label: powerpipe benchmark
---


# powerpipe benchmark

List, view, and run Powerpipe benchmarks.

## Usage
```bash
powerpipe benchmark list [args]
powerpipe benchmark show benchmark_name [args]
powerpipe benchmark run benchmark_name [args]
```



## Sub-Commands

| Command | Description
|-|-
| `list` | List benchmarks from the current mod and its direct dependents.
| `run`  | Run a benchmark from the current mod or its direct dependents or from a Powerpipe server instance.
| `show` | Show details of a benchmark from the current mod or its direct dependents.



## Arguments

| Flag | Applies to | Description
|-|-|-
|  `--cloud-host`                 | `run` | Sets the Turbot Pipes host used when connecting to Turbot Pipes workspaces. See `POWERPIPE_CLOUD_HOST` for details.
|  `--cloud-token`                | `run` | Sets the Turbot Pipes authentication token used when connecting to Turbot Pipes workspaces. See `POWERPIPE_CLOUD_TOKEN` for details.
| `--detach`   | `run` | Start the benchmark and return immediately.  By default, `powerpipe benchmark run` will run the benchmark and wait for the results. You may only use `--detach` when running a benchmark from a server instance (by specifying `--host`, for example).
|  `--dry-run`                    | `run` | If specified, prints the controls that would be run by the command, but does not execute them.
|  `--export string`              | `run` | Export control output to a file. You may export multiple output formats for a single control run by entering multiple `--export` arguments. If a file path is specified as an argument, its type will be inferred by the suffix. Supported export formats are `asff`, `csv`, `html`, `json`, `md`,`nunit3`, `sps` (snapshot)
|  `--header string`              | `run` | Specify whether to include column headers in csv output/export (default `true`).
|  `--help`                       | `run` | Help for steampipe check.
|  `--input`                      | `run` | Enable/Disable interactive prompts for missing variables. To disable prompts and fail on missing variables, use  `--input=false`. This is useful when running from scripts. (default `true`)
|  `--max-parallel int`           | `run` | Set the maximum number of database connections to open. When running benchmarks, Powerpipe will attempt to run up to this many controls in parallel. See the `POWERPIPE_MAX_PARALLEL` environment variable documentation for details. (default `10`)
|  `--mod-install`                | `run` | Specify whether to install mod dependencies before running the benchmark (default `true`)
|  `--mod-location`               | `run` | Sets the Powerpipe workspace working directory. If not specified, the workspace directory will be set to the current working directory. See `POWERPIPE_MOD_LOCATION` for details.
|  `--output string`              | `run` | Select the console output format. Defaults to text. Possible values are `brief`, `csv`, `html`, `json`, `md`, `sps` (snapshot), `pretty`, `plain`, `none`
|  `--progress`                   | `run` | Enable or disable progress information. By default, progress information is shown - set  `--progress=false` to hide the progress bar.
|  `--query-timeout int`          | `run` | The query timeout, in seconds. The default is `240`.
|  `--search-path strings`        | `run` | Set a comma-separated list of connections to use as a custom search path for the control run.
|  `--search-path-prefix strings` | `run` | Set a comma-separated list of connections to use as a prefix to the current search path for the control run.
|  `--separator string`           | `run` | A single character to use as a separator string for csv output (defaults to `,`)
|  `--share`                      | `run` | Create snapshot in Turbot Pipes with `anyone_with_link` visibility.
|  `--snapshot`                   | `run` | Create snapshot in Turbot Pipes with the default (`workspace`) visibility.
|  `--snapshot-location string`   | `run` |	The location to write snapshots - either a local file path or a Turbot Pipes workspace
|  `--snapshot-tag string=string` | `run` | Specify tags to set on the snapshot. Multiple `--snapshot-tag `arguments may be passed.
|  `--snapshot-title string=string` | `run` | The title to give a snapshot when uploading to Turbot Pipes.
|  `--tag string=string`          | `run` | Filter the list of controls to run by one or more tag values. Multiple `--tag `arguments may be passed. Discrete keys are and'ed and duplicate keys are or'ed. For example, `steampipe check all --tag pci=true --tag service=ec2 --tag service=iam` will run only controls with a `service` tag equal to either `ec2` or `iam` that also are tagged with `pci=true`.
|  `--theme string`               | `run` | Select output theme (color scheme, etc). Defaults to `dark`. Possible values are `light`,`dark`, `plain`
|  `--timing`                     | `run` | Turn on the query timer.
| `--var string=string` | `run`| Specify the value of a variable.  Multiple `--var` arguments may be passed. 
| `--var-file strings`| `run`| Specify a `.ppvar` file containing variable values.
|  `--where string`               | `run` | Filter the list of controls to run, using a sql where clause against the steampipe_control reflection table.
|  `--workspace-database`         | `run` | Sets the database that Steampipe will connect to. This can be local (the default) or a remote Turbot Pipes database. See POWERPIPE_WORKSPACE_DATABASE for details.



## Output Formats
| Format | Description 
|-|-
| `asff` | [Findings](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-findings.html) in [asff](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-findings-format-syntax.html) json format. Only used with AWS controls.
| `brief` | Text based output that shows only actionable items (errors and alarms) as well as a summary.
| `csv` | Comma-separated output with full control details.
| `html` | Single-page HTML output with full control details and group summaries.
| `json` | Hierarchical json output with full control details and group summaries.
| `md` | Single-page markdown output with full control details and group summaries.
| `none` | Don't send any output to stdout.
| `nunit3` | Results in [nunit3](https://docs.nunit.org/articles/nunit/technical-notes/usage/Test-Result-XML-Format.html) xml format.
| `snapshot` | Steampipe snapshot json (alias for `sps`)
| `sps` | Steampipe snapshot json.
| `pretty` | Full text based output with details and summary.  This is the default console output format.
| `plain` | Full text based output with details and summary, without color.

## Examples


List benchmarks.  By default only top-level dashboards are shown:
```bash
powerpipe benchmark list
```

List All benchmarks, including sub-benchmarks:
```bash
powerpipe benchmark list --all
```

List benchmark in `JSON` format:
```bash
powerpipe benchmark list --output json
powerpipe benchmark list --all --output json
```

Show details of a single benchmark:
```bash
powerpipe benchmark show cis_v120
powerpipe benchmark show benchmark.cis_v120
```

Show details of a single benchmark in a direct dependency mod:
```bash
powerpipe benchmark show aws_compliance.benchmark.cis_v120
```

Run a benchmark 
```bash
powerpipe benchmark run cis_v120
```

Run multiple benchmarks
```bash
powerpipe benchmark run cis_v120 cis_v150
```


Run a benchmark, only including controls with specific property values:
```bash
powerpipe benchmark run cis_v120 --where "severity in ('critical', 'high')"
```

Run a benchmark, only including controls with specific tags:
```bash
powerpipe benchmark run cis_v120  --tag cis_level=1 --tag cis=true
```


Run a benchmark and upload a snapshot with `workspace` visibility in your user workspace.
```bash
powerpipe benchmark run --snapshot cis_v120 
```


Run a benchmark and upload a snapshot with `anyone_with_link` visibility in your user workspace.
```bash
powerpipe benchmark run --share cis_v120 
```


Run a benchmark and upload a snapshot with `anyone_with_link` visibility to specific workspace.
```bash
powerpipe benchmark run --share  --snapshot-location vandelay-industries/latex cis_v120
```

Run a benchmark, upload a snapshot with `workspace` visibility in your user workspace, and tag the snapshot:
```bash
powerpipe benchmark run --snapshot --snapshot-tag env=local cis_v120 
```

