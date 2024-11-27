---
title: powerpipe query
---


# powerpipe query

List, view, and run Powerpipe queries.  You can run [named queries](/docs/powerpipe-hcl/query) or any arbitrary SQL.

## Usage
```bash
powerpipe query list [args]
powerpipe query show query_name [args]
powerpipe query run query_name [args]
powerpipe query run "select * from my_table" [args]
```


## Sub-Commands

| Command | Description
|-|-
| [list](#powerpipe-query-list) | List queries from the current mod and its direct dependents.
| [run](#powerpipe-query-run)  | Run a query from the current mod or its direct dependents.
| [show](#powerpipe-query-show) | Show details of a query from the current mod or its direct dependents.



----
## powerpipe query list
List queries from the current mod and its direct dependents.

### Examples

List queries:
```bash
powerpipe query list
```


List all queries in `JSON` format:
```bash
powerpipe query list --output json
```

List queries using settings from a workspace:
```bash
powerpipe query list --workspace my_workspace
```

---

## powerpipe query show
Show details of a query from the current mod or its direct dependents.


### Examples

Show details of a single query in the current mod:
```bash
powerpipe query show ec2_instance_in_vpc
# or
powerpipe query show query.ec2_instance_in_vpc
```


Show details of a single query in a direct dependency mod:
```bash
powerpipe query show aws_compliance.query.ec2_instance_in_vpc
```

Show details of a query in `JSON` format:
```bash
powerpipe query show ec2_instance_in_vpc --output json
```


Show details of a query using settings from a workspace:
```bash
powerpipe query show ec2_instance_in_vpc -workspace my_workspace
```
---

## powerpipe query run
Run a query from the current mod or its direct dependents.

### Arguments

| Flag | Description
|-|-
| `--arg string=string`           | Specify the value for a query param. Multiple `--arg` arguments may be passed. 
|  `--database`                   |  ***DEPRECATED - See [Setting the Database](/docs/build/mod-database) for the new syntax.***  Sets the [database that Powerpipe will connect to](/docs/run#selecting-a-database). This defaults to the local Steampipe database, but can be any PostgreSQL, MySQL, DuckDB, or SQLite database.
|  `--export string`              | Export query output to a file. You may export multiple output formats for a single query run by entering multiple `--export` arguments. If a file path is specified as an argument, its type will be inferred by the suffix. Supported export formats are  `csv`, `line`, `json`,`pps` (snapshot), `pretty`, `plain`.
|  `--header string`              | Specify whether to include column headers in csv output/export (default `true`).
|  `--input`                      | Enable/Disable interactive prompts for missing variables. To disable prompts and fail on missing variables, use  `--input=false`. This is useful when running from scripts. (default `true`)
|  `--mod-install`                | Specify whether to install mod dependencies before running the query (default `true`)
|  `--output string`              | Select the console output format. Defaults to text. Possible values are `csv`, `line`, `json`,`pps` (snapshot), `pretty`, `plain`.
|  `--pipes-host`                 | Sets the Turbot Pipes host used when connecting to Turbot Pipes workspaces. See  [PIPES_HOST](/docs/reference/env-vars/pipes_host) for details.
|  `--pipes-token`                | Sets the Turbot Pipes authentication token used when connecting to Turbot Pipes workspaces. See  [PIPES_TOKEN](/docs/reference/env-vars/pipes_token) for details.
|  `--progress`                   | Enable or disable progress information. By default, progress information is shown - set  `--progress=false` to hide the progress bar.
|  `--query-timeout int`          | The query timeout, in seconds. The default is `300`.
|  `--search-path strings`        | Set a comma-separated list of connections to use as a custom search path for the query run.
|  `--search-path-prefix strings` | Set a comma-separated list of connections to use as a prefix to the current search path for the query run.
|  `--separator string`           | A single character to use as a separator string for csv output (defaults to `,`)
|  `--share`                      | Create snapshot in Turbot Pipes with `anyone_with_link` visibility.
|  `--snapshot`                   | Create snapshot in Turbot Pipes with the default (`workspace`) visibility.
|  `--snapshot-location string`   |	The location to write snapshots - either a local file path or a Turbot Pipes workspace
|  `--snapshot-tag string=string` | Specify tags to set on the snapshot. Multiple `--snapshot-tag `arguments may be passed.
|  `--snapshot-title string=string` | The title to give a snapshot when uploading to Turbot Pipes.
|  `--timing`                     | Turn on the query timer.
| `--var string=string`           | Specify the value of a variable.  Multiple `--var` arguments may be passed. 
| `--var-file strings`            | Specify a `.ppvar` file containing variable values.



### Examples

Run a named query:
```bash
powerpipe query run ec2_instance_in_vpc
# or
powerpipe query run query.ec2_instance_in_vpc
```

Run an ad-hoc query:
```bash
powerpipe query run "select * from aws_account"
```

Run a query and return CSV:
```bash
powerpipe query run "select * from aws_account" --output csv
```

Run a query and export results to a CSV file:
```bash
powerpipe query run "select * from aws_account" --export csv
```

Run a query and export results to a CSV file with a specific name:
```bash
powerpipe query run ec2_instance_in_vpc --export myfile.csv
```

Run a query and export results in multiple formats:
```bash
powerpipe query run  "select * from aws_account" --export results.csv --export results.json
```

Run a named query and pass arguments:
```bash
powerpipe query run check_vpc --arg vpc_ids='["vpc-12345678","vpc-22222222"]' --arg account_id='012345678901'
```

Run an ad-hoc query and create a snapshot file:
```bash
powerpipe query run "select * from aws_account" --export my_snap.pps
# or
powerpipe query run "select * from aws_account" --output pps > my_snap.pps
```

Run a query against a pipes workspace:
```bash
powerpipe query run ec2_instance_in_vpc --workspace acme/anvils
```

Run a query and upload a snapshot with `workspace` visibility in your user workspace.
```bash
powerpipe query run ec2_instance_in_vpc --snapshot 
```


Run a query and upload a snapshot with `anyone_with_link` visibility in your default workspace.
```bash
powerpipe query run "select * from aws_account" --share 
```

Run a query and upload a snapshot with `anyone_with_link` visibility to a specific workspace.
```bash
powerpipe query run ec2_instance_in_vpc --share  --snapshot-location vandelay-industries/latex
```

Run a query, upload a snapshot with `workspace` visibility in your user workspace, and tag the snapshot:
```bash
powerpipe query run ec2_instance_in_vpc --snapshot --snapshot-tag env=local 
```

Run a query with a specific search_path:
```bash
powerpipe query run "select * from aws_s3_bucket" --search-path="aws_dmi,github,slack" 
```

Run a query with a specific search_path_prefix:
```bash
powerpipe query run "select * from aws_s3_bucket" --search-path-prefix="aws_dmi" 
```

