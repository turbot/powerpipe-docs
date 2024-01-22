---
title: powerpipe dashboard
sidebar_label: powerpipe dashboard
---


# powerpipe dashboard

List, view, and run Powerpipe dashboards in batch mode.  To run dashboards interactively, see [powerpipe server](/docs/reference/cli/server).

## Usage

```bash
powerpipe dashboard list [args]
powerpipe dashboard show dashboard_name [args]
powerpipe dashboard run dashboard_name [args]

powerpipe dashboard card list [args]
powerpipe dashboard chart list [args]
powerpipe dashboard container list [args]
powerpipe dashboard flow list [args]
powerpipe dashboard graph list [args]
powerpipe dashboard hierarchy list [args]
powerpipe dashboard image list [args]
powerpipe dashboard input list [args]
powerpipe dashboard table list [args]
powerpipe dashboard text list [args]
```


## Sub-Commands

| Command | Description
|-|-
| [list](#powerpipe-dashboard-list) | List dashboards from the current mod and its direct dependents.
| [run](#powerpipe-dashboard-run)  | Run a dashboard from the current mod or its direct dependents or from a Powerpipe server instance.
| [show](#powerpipe-dashboard-show) | Show details of a dashboard from the current mod or its direct dependents or from a Powerpipe server instance.
| [[resource type] list](#powerpipe-dashboard-resource-type-list) | List dashboard resources of a given type from the current mod and its direct dependents.



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

List dashboards from a local server instance running on the default port on `localhost`:
```bash
powerpipe dashboard list --host local
```

List dashboards on a remote Powerpipe server instance:
```bash
powerpipe dashboard list --host  https://powerpipe.my-org.com:9194
```


List dashboards using settings from a workspace:
```bash
powerpipe dashboard list --workspace my_workspace
```


---

## powerpipe dashboard show
Show details of a dashboard from the current mod or its direct dependents or from a Powerpipe server instance.


### Examples

Show details of a single dashboard in the current mod:
```bash
powerpipe dashboard show cis_v200_2_1_1
# or
powerpipe dashboard show dashboard.cis_v200_2_1_1
```


Show details of a single dashboard in a direct dependency mod:
```bash
powerpipe dashboard show aws_compliance.dashboard.cis_v200_2_1_1
```

Show details of a dashboard on a Powerpipe server instance:
```bash
powerpipe dashboard show aws_compliance.dashboard.cis_v200_2_1_1 --host https://powerpipe.my-org.com:9194
```


Show details of a dashboard in `JSON` format:
```bash
powerpipe dashboard show cis_v200_2_1_1 --output json
```


Show details of a dashboard using settings from a workspace:
```bash
powerpipe dashboard show cis_v200_2_1_1 -workspace my_workspace
```
---

## powerpipe dashboard run
Run a dashboard from the current mod or its direct dependents or from a Powerpipe server instance.

### Arguments

| Flag | Description
|-|-
| `--arg string=string`           | Specify the value for a dashboard input. Multiple `--arg` arguments may be passed. 
|  `--cloud-host`                 | Sets the Turbot Pipes host used when connecting to Turbot Pipes workspaces. See `POWERPIPE_CLOUD_HOST` for details.
|  `--cloud-token`                | Sets the Turbot Pipes authentication token used when connecting to Turbot Pipes workspaces. See `POWERPIPE_CLOUD_TOKEN` for details.
|  `--export string`              | Export dashboard output to a file. You may export multiple output formats for a single dashboard run by entering multiple `--export` arguments. If a file path is specified as an argument, its type will be inferred by the suffix. Supported export formats are `none`, `sps` (`snapshot`)
|  `--input`                      | Enable/Disable interactive prompts for missing variables. To disable prompts and fail on missing variables, use  `--input=false`. This is useful when running from scripts. (default `true`)
|  `--max-parallel int`           | Set the maximum number of database connections to open. When running dashboards, Powerpipe will attempt to run up to this many dashboards in parallel. See the `POWERPIPE_MAX_PARALLEL` environment variable documentation for details. (default `10`)
|  `--mod-install`                | Specify whether to install mod dependencies before running the dashboard (default `true`)
|  `--output string`              | Select the console output format. Defaults to text. Possible values are `none`, `sps` (`snapshot`)
|  `--progress`                   | Enable or disable progress information. By default, progress information is shown - set  `--progress=false` to hide the progress bar.
|  `--query-timeout int`          | The query timeout, in seconds. The default is `240`.
|  `--search-path strings`        | Set a comma-separated list of connections to use as a custom search path for the dashboard run.
|  `--search-path-prefix strings` | Set a comma-separated list of connections to use as a prefix to the current search path for the dashboard run.
|  `--share`                      | Create snapshot in Turbot Pipes with `anyone_with_link` visibility.
|  `--snapshot`                   | Create snapshot in Turbot Pipes with the default (`workspace`) visibility.
|  `--snapshot-location string`   |	The location to write snapshots - either a local file path or a Turbot Pipes workspace
|  `--snapshot-tag string=string` | Specify tags to set on the snapshot. Multiple `--snapshot-tag `arguments may be passed.
|  `--snapshot-title string=string` | The title to give a snapshot when uploading to Turbot Pipes.
| `--var string=string`           | Specify the value of a variable.  Multiple `--var` arguments may be passed. 
| `--var-file strings`            | Specify a `.ppvar` file containing variable values.
|  `--database`         | Sets the database that Powerpipe will connect to. This can be local (the default) or a remote Turbot Pipes database. See [POWERPIPE_DATABASE](/docs/reference/env-vars/powerpipe_database) for details.



### Examples

Run a dashboard and output the snapshot data:
```bash
powerpipe dashboard run cis_v200_2_1_1
```

Run a dashboard and output the snapshot data to a file:
```bash
powerpipe dashboard run cis_v200_2_1_1 > mysnap.sps
# or
powerpipe dashboard run cis_v200_2_1_1 --export mysnap.sps
```


Run a dashboard and upload a snapshot with `workspace` visibility in your user workspace.
```bash
powerpipe dashboard run cis_v200_2_1_1 --snapshot  
```

Run a dashboard and upload a snapshot with `anyone_with_link` visibility in your user workspace.
```bash
powerpipe dashboard run --share cis_v200_2_1_1 
```

Run a dashboard and save a [snapshot](/docs/run/snapshots/batch-snapshots), specifying inputs:

```bash
powerpipe dashboard run aws_insights.dashboard.aws_vpc_detail \
  --snapshot \
  --dashboard-input vpc_id=vpc-9d7ae1e7
```


Run a dashboard and upload a snapshot with `anyone_with_link` visibility to specific workspace.
```bash
powerpipe dashboard run cis_v200_2_1_1 --share  --snapshot-location vandelay-industries/latex 
```

Run a dashboard, upload a snapshot with `workspace` visibility in your user workspace, and tag the snapshot:
```bash
powerpipe dashboard run cis_v200_2_1_1 --snapshot --snapshot-tag env=local 
```

Run a dashboard on a remote powerpipe host
```bash
powerpipe dashboard run cis_v200_2_1_1 --host https://powerpipe.my-org.com:9194
```

Run a dashboard against a pipes workspace:
```bash
powerpipe dashboard run cis_v200_2_1_1 --workspace acme/anvils
```

Run a dashboard against a specific database:
```bash
powerpipe dashboard run cis_v200_2_1_1 --database  postgres://myusername:passworrd@mydbserver.mydomain.com:9193/steampipe
```





## powerpipe dashboard [resource type] list
List dashboard resources of a given type from the current mod and its direct dependents.

### Examples


List dashboard [cards](/docs/powerpipe-hcl/card):
```bash
powerpipe dashboard card list
```

List dashboard [charts](/docs/powerpipe-hcl/chart):
```bash
powerpipe dashboard chart list
```

List dashboard [containers](/docs/powerpipe-hcl/container):
```bash
powerpipe dashboard container list
```

List dashboard [flows](/docs/powerpipe-hcl/flow):
```bash
powerpipe dashboard flow list
```

List dashboard [graphs](/docs/powerpipe-hcl/graph):
```bash
powerpipe dashboard graph list
```

List dashboard [hierarchies](/docs/powerpipe-hcl/hierarchy):
```bash
powerpipe dashboard hierarchy list
```

List dashboard [images](/docs/powerpipe-hcl/image):
```bash
powerpipe dashboard image list
```

List dashboard [inputs](/docs/powerpipe-hcl/input):
```bash
powerpipe dashboard input list
```

List dashboard [tables](/docs/powerpipe-hcl/table):
```bash
powerpipe dashboard table list
```

List dashboard [texts](/docs/powerpipe-hcl/text):
```bash
powerpipe dashboard text list
```

List dashboard cards in`JSON` format:
```bash
powerpipe dashboard card list --output json
```

List dashboard cards from a local server instance running on the default port on `localhost`:
```bash
powerpipe dashboard card list --host local
```

List dashboard cards on a remote Powerpipe server instance:
```bash
powerpipe dashboard card list --host  https://powerpipe.my-org.com:9194
```

