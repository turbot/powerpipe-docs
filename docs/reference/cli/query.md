---
title: powerpipe query
sidebar_label: powerpipe query
---


# powerpipe query

List, view, and run Powerpipe queries.  You can run named queries or any arbitrary SQL.

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
| [run](#powerpipe-query-run)  | Run a query from the current mod or its direct dependents or from a Powerpipe server instance.
| [show](#powerpipe-query-show) | Show details of a query from the current mod or its direct dependents or from a Powerpipe server instance.



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

List queries from a local server instance running on the default port on `localhost`:
```bash
powerpipe query list --host local
```


List queries on a remote Powerpipe server instance:
```bash
powerpipe query list --host  https://powerpipe.my-org.com:9194
```


List queries using settings from a workspace:
```bash
powerpipe query list --workspace my_workspace
```


---

## powerpipe query show
Show details of a query from the current mod or its direct dependents or from a Powerpipe server instance.


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

Show details of a query on a Powerpipe server instance:
```bash
powerpipe query show aws_compliance.query.ec2_instance_in_vpc --host https://powerpipe.my-org.com:9194
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
Run a query from the current mod or its direct dependents or from a Powerpipe server instance.

### Arguments

| Flag | Description
|-|-
| `--arg string=string`           | Specify the value for a query param. Multiple `--arg` arguments may be passed. 
|  `--cloud-host`                 | Sets the Turbot Pipes host used when connecting to Turbot Pipes workspaces. See `POWERPIPE_CLOUD_HOST` for details.
|  `--cloud-token`                | Sets the Turbot Pipes authentication token used when connecting to Turbot Pipes workspaces. See `POWERPIPE_CLOUD_TOKEN` for details.
|  `--export string`              | Export query output to a file. You may export multiple output formats for a single query run by entering multiple `--export` arguments. If a file path is specified as an argument, its type will be inferred by the suffix. Supported export formats are  `csv`, `line`, `json`,`sps` (snapshot), `pretty`, `plain`.
|  `--header string`              | Specify whether to include column headers in csv output/export (default `true`).
|  `--input`                      | Enable/Disable interactive prompts for missing variables. To disable prompts and fail on missing variables, use  `--input=false`. This is useful when running from scripts. (default `true`)
|  `--mod-install`                | Specify whether to install mod dependencies before running the query (default `true`)
|  `--output string`              | Select the console output format. Defaults to text. Possible values are `csv`, `line`, `json`,`sps` (snapshot), `pretty`, `plain`.
|  `--progress`                   | Enable or disable progress information. By default, progress information is shown - set  `--progress=false` to hide the progress bar.
|  `--query-timeout int`          | The query timeout, in seconds. The default is `240`.
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
|  `--workspace-database`         |  Sets the database that Powerpipe will connect to. This can be local (the default) or a remote Turbot Pipes database. See [POWERPIPE_WORKSPACE_DATABASE](/docs/reference/env-vars/powerpipe_workspace_database) for details.



### Examples

Run a named query:
```bash
powerpipe query run ec2_instance_in_vpc
# or
powerpipe query run query.ec2_instance_in_vpc
```

Run an adhoc query:
```bash
powerpipe query run "select * from aws_account"
```


Run an adhoc query and create a snapshot file:
```bash
powerpipe query run "select * from aws_account" --export my_snap.sps
# or
powerpipe query run "select * from aws_account" --output sps > my_snap.sps
```

Run a named query on a remote powerpipe host
```bash
powerpipe query run ec2_instance_in_vpc --host  https://powerpipe.my-org.com:9194
```

Run an adhoc query on a remote powerpipe host
```bash
powerpipe query run "select * from aws_account" --host  https://powerpipe.my-org.com:9194
```


Run a query against a pipes workspace:
```bash
powerpipe query run ec2_instance_in_vpc --workspace acme/anvils
```

Run a query against a specific database:
```bash
powerpipe query run "select * from aws_account" --workspace-database  postgres://myusername:passworrd@mydbserver.mydomain.com:9193/steampipe
```

Run a query and upload a snapshot with `workspace` visibility in your user workspace.
```bash
powerpipe query run ec2_instance_in_vpc --snapshot 
```


Run a query and upload a snapshot with `anyone_with_link` visibility in your default workspace.
```bash
powerpipe query run "select * from aws_account" --share 
```

Run a query and upload a snapshot with `anyone_with_link` visibility to specific workspace.
```bash
powerpipe query run ec2_instance_in_vpc --share  --snapshot-location vandelay-industries/latex
```

Run a query, upload a snapshot with `workspace` visibility in your user workspace, and tag the snapshot:
```bash
powerpipe query run ec2_instance_in_vpc --snapshot --snapshot-tag env=local 
```




----


---



----



# powerpipe query
Execute SQL queries interactively, or by a query argument.

To open the interactive query shell, run `powerpipe query` with no arguments.  The query shell provides a way to explore your data and run multiple queries. 

If a query string is passed on the command line then it will be run immediately and the command will exit.  Alternatively, you may specify one or more files containing SQL statements.  You can run multiple SQL files by passing a glob or a space separated list of file names.

If the Powerpipe service was previously started by `powerpipe service start`, powerpipe will connect to the service instance - otherwise, the query command will start the `service`. At the end of the query command or session, if other sessions have not connected to the `service` already, the `service` will be shutdown. If other session have already connected to the `service`, then the last session to exit will shutdown the `service`.

## Usage
Run Powerpipe [interactive query shell](/docs/query/query-shell):
```bash
powerpipe query [flags]
```

Run a [batch query](/docs/query/batch-query):
```bash
powerpipe query {query} [flags]
```

List available [named queries](/docs/query/batch-query#named-queries):
```bash
powerpipe query list
```


## Flags

| Flag | Description
|-|-


<table>
  <tr> 
    <th> Argument </th> 
    <th> Description </th> 
  </tr>
  <tr> 
    <td nowrap="true"> <inlineCode>--cloud-host</inlineCode> </td> 
    <td>  Sets the Turbot Pipes host used when connecting to Turbot Pipes workspaces. See <a href="reference/env-vars/powerpipe_cloud_host">STEAMPIPE_CLOUD_HOST</a> for details.</td> 
  </tr>

  <tr> 
    <td nowrap="true"> <inlineCode>--cloud-token</inlineCode> </td> 
    <td>  Sets the Turbot Pipes authentication token used when connecting to Turbot Pipes workspaces. See <a href="reference/env-vars/powerpipe_cloud_token">STEAMPIPE_CLOUD_TOKEN</a> for details.</td> 
  </tr>

  <tr> 
    <td nowrap="true"> <inlineCode>--export string</inlineCode>  </td> 
    <td> Export query output to a file.  You may export multiple output formats by entering multiple <inlineCode>--export</inlineCode> arguments.  If a file path is specified as an argument, its type will be inferred by the suffix.  Supported export formats are  <inlineCode>sps</inlineCode> (<inlineCode>snapshot</inlineCode>).
    </td> 

  </tr>


  <tr> 
    <td nowrap="true"> <inlineCode>--header string</inlineCode>  </td> 
    <td> Specify whether to include column headers in csv and table output (default <inlineCode>true</inlineCode>).</td> 
  </tr>

  <tr> 
    <td nowrap="true"> <inlineCode>--help</inlineCode> </td> 
    <td>  Help for <inlineCode>powerpipe query.</inlineCode></td> 
  </tr>

  <tr> 
    <td nowrap="true"> <inlineCode>--input</inlineCode> </td> 
    <td>  Enable/Disable interactive prompts for missing variables.  To disable prompts and fail on missing variables, use <inlineCode>--input=false</inlineCode>.  This is useful when running from scripts. (default true)</td> 
  </tr>

  <tr> 
    <td nowrap="true"> <inlineCode>--mod-location </inlineCode> </td> 
    <td>  Sets the Powerpipe workspace working directory. If not specified, the workspace directory will be set to the current working directory. See <a href="reference/env-vars/powerpipe_mod_location">STEAMPIPE_MOD_LOCATION</a> for details. </td> 
  </tr>

  <tr> 
    <td nowrap="true"> <inlineCode>--output string</inlineCode> </td> 
    <td>  Select the console output format.   Possible values are <inlineCode>line, csv, json, table, snapshot</inlineCode> (default <inlineCode>table) </inlineCode>. </td> 
  </tr>

  <tr> 
    <td nowrap="true"> <inlineCode>--progress</inlineCode>  </td> 
    <td> Enable or disable progress information. By default, progress information is shown - set <inlineCode>--progress=false</inlineCode> to hide the progress bar.  </td>
  </tr>

  <tr> 
    <td nowrap="true"> <inlineCode>--query-timeout int</inlineCode>  </td> 
    <td>  The query timeout, in seconds.  The default is <inlineCode>0</inlineCode>  (no timeout).  </td>
  </tr>

  <tr> 
    <td nowrap="true"> <inlineCode>--search-path strings</inlineCode>  </td> 
    <td>  Set a comma-separated list of connections to use as a custom <a href="managing/connections#setting-the-search-path">search path</a> for the query session. </td>
  </tr>
      <tr> 
    <td nowrap="true"> <inlineCode>--search-path-prefix strings</inlineCode>  </td> 
    <td>  Set a comma-separated list of connections to use as a prefix to the current <a href="managing/connections#setting-the-search-path">search path</a> for the query session. </td>
  </tr>
  <tr> 
    <td nowrap="true"> <inlineCode>--separator string</inlineCode>  </td> 
    <td>  A single character to use as a separator string for csv output (defaults to  ",")  </td>
  </tr>


  <tr> 
    <td nowrap="true"> <inlineCode>--share</inlineCode>  </td> 
    <td> Create snapshot in Turbot Pipes with <inlineCode>anyone_with_link</inlineCode> visibility.  </td>
  </tr>

  <tr> 
    <td nowrap="true"> <inlineCode>--snapshot</inlineCode>  </td> 
    <td> Create snapshot in Turbot Pipes with the default (<inlineCode>workspace</inlineCode>) visibility.  </td>
  </tr>
    
  <tr> 
    <td nowrap="true"> <inlineCode>--snapshot-location string</inlineCode>  </td> 
    <td> The location to write snapshots - either a local file path or a Turbot Pipes workspace  </td>
  </tr>

  <tr> 
    <td nowrap="true"> <inlineCode>--snapshot-tag string=string  </inlineCode>  </td> 
    <td> Specify tags to set on the snapshot.  Multiple <inlineCode>--snapshot-tag </inlineCode> arguments may be passed.</td>
  </tr>


  <tr> 
    <td nowrap="true"> <inlineCode>--snapshot-title string=string  </inlineCode>  </td> 
    <td> The title to give a snapshot when uploading to Turbot Pipes.  </td>
  </tr>




  <tr> 
    <td nowrap="true"> <inlineCode>--timing  </inlineCode>  </td> 
    <td>Turn on the query timer.  </td>
  </tr>



  <tr> 
    <td nowrap="true"> <inlineCode>--var string=string </inlineCode>  </td> 
    <td>  Specify the value of a mod variable.  Multiple <inlineCode>--var </inlineCode> arguments may be passed.
    </td>
  </tr>
  <tr> 
    <td nowrap="true"> <inlineCode>--var-file string</inlineCode>  </td> 
    <td>  Specify an .spvars file containing mod variable values. 
    </td>
  </tr>

  <tr> 
    <td nowrap="true"> <inlineCode>--watch</inlineCode>  </td> 
    <td> Watch SQL files in the current workspace (works only in interactive mode) (default true)
    </td>
  </tr>

  <tr> 
    <td nowrap="true"> <inlineCode>--workspace-database</inlineCode>  </td> 
    <td>  Sets the database that Powerpipe will connect to. This can be <inlineCode>local</inlineCode> (the default) or a remote Turbot Pipes database.  See <a href="/docs/reference/env-vars/powerpipe_workspace_database">STEAMPIPE_WORKSPACE_DATABASE</a> for details. </td>
  </tr>
</table>




## Examples

Open an interactive query console:
```bash
powerpipe query
```

Run a specific query directly:
```bash
powerpipe query "select * from aws_s3_bucket"
```

Run a query and save a [snapshot](/docs/snapshots/batch-snapshots):
```bash
powerpipe query --snapshot "select * from aws_s3_bucket"
```

Run a query and share a [snapshot](/docs/snapshots/batch-snapshots):
```bash
powerpipe query --share "select * from aws_s3_bucket"
```

List the named queries available to run in the current mod context:

```bash
powerpipe query list
```

Run a named query:
```bash
powerpipe query query.s3_bucket_logging_enabled
```


Run the SQL command in the `my_queries/my_query.sql` file:
```bash
powerpipe query my_queries/my_query.sql
```

Run the SQL commands in all `.sql` files in the `my_queries` directory and concatenate the results:
```bash
powerpipe query my_queries/*.sql
```

Run a specific query directly and report the query execution time:
```bash
powerpipe query "select * from aws_s3_bucket" --timing
```

Run a specific query directly and return output in json format:
```bash
powerpipe query "select * from aws_s3_bucket" --output json
```

Run a specific query directly and return output in CSV format:
```bash
powerpipe query "select * from aws_s3_bucket" --output csv
```

Run a specific query directly and return output in pipe-separated format:
```bash
powerpipe query "select * from aws_s3_bucket" --output csv --separator '|'
```


Run a query with a specific search_path:
```bash
powerpipe query --search-path="aws_dmi,github,slack" "select * from aws_s3_bucket"
```

Run a query with a specific search_path_prefix:
```bash
powerpipe query --search-path-prefix="aws_dmi" "select * from aws_s3_bucket"
```