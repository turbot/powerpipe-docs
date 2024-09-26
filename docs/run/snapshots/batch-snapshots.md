---
title: Batch Snapshots
sidebar_label: Batch Snapshots
---

# Taking Snapshots from the Command Line

*To upload snapshots to Turbot Pipes, you must either [log in via the `powerpipe login` command](/docs/reference/cli/login) or create an [API token](https://turbot.com/pipes/docs/da-settings#tokens) and pass it via the [`--pipes-token`](/docs/reference/cli#global-flags) argument or [`PIPES_TOKEN`](/docs/reference/env-vars/pipes_token) environment variable.*

To take a snapshot and save it to [Turbot Pipes](https://turbot.com/pipes/docs), simply add the `--snapshot` flag to your `run` command.  

You can take a snapshot of a dashboard:
```bash
powerpipe dashboard run aws_insights.dashboard.aws_account_report --snapshot 
```

or a benchmark:

```bash
powerpipe benchmark run benchmark.cis_v140 --snapshot 
```

or a control:

```bash
powerpipe control run cis_v140_2_1_1 --snapshot 
```


or a query:

```bash
powerpipe query run "select * from aws_ec2_instance" --snapshot
```

including named queries:

```bash
powerpipe query aws_compliance.query.vpc_network_acl_unused  --snapshot
```


## Sharing Snapshots

The `--snapshot` flag will create a snapshot with `workspace` visibility in your user workspace. A snapshot with `workspace` visibility is visible only to users who have access to the workspace in which the snapshot resides -- A user must be authenticated to Turbot Pipes with permissions on the workspace.

If you want to create a snapshot that can be shared with *anyone*, use the `--share` flag instead. This will create the snapshot with `anyone_with_link` visibility:

```bash
powerpipe dashboard run aws_insights.dashboard.aws_account_report --share
```


You can set a snapshot title in Turbot Pipes with the `--snapshot-title` argument.  This is especially useful for ad hoc queries:

```bash
powerpipe query run "select name from aws_s3_bucket where bucket_policy_is_public" \
  --share \
  --snapshot-title "Public Buckets" 
```


If you wish to save the snapshot to a different workspace, such as an org workspace, you can use the `--snapshot-location` argument with `--share` or `--snapshot`:

```bash
powerpipe benchmark run benchmark.cis_v140 \
  --snapshot \
  --snapshot-location vandelay-industries/latex
```

Note that the previous command ran the benchmark against the *local* database, but saved the snapshot to the `vandelay-industries/latex` workspace.  If you want to run the benchmark against the remote `vandelay-industries/latex` database AND store the snapshot there, you can also add the `--database-location` argument:

```bash
powerpipe benchmark run benchmark.cis_v140  \
  --snapshot \
  --snapshot-location vandelay-industries/latex \
  --database vandelay-industries/latex
```

Powerpipe provides a shortcut for this though.  The `--workspace` flag supports [passing the cloud workspace](/docs/run/workspaces#implicit-workspaces):
```bash
powerpipe benchmark run benchmark.cis_v140 \
  --snapshot \
  --workspace vandelay-industries/latex
```

While not a common case, you can even run a benchmark against a Turbot Pipes workspace database, but store the snapshot in an entirely different Turbot Pipes workspace:
```bash
powerpipe benchmark run benchmark.cis_v140  
  --snapshot \
  --database vandelay-industries/latex-dev \
  --snapshot-location  vandelay-industries/latex-prod 
```

## Passing Args

If your dashboard has [inputs](/docs/powerpipe-hcl/input), you may specify them with one or more `--arg` arguments:

```bash
powerpipe dashboard run aws_insights.dashboard.aws_vpc_detail\
  --snapshot \
  --arg vpc_id=vpc-9d7ae1e7
```

Likewise, if you want to run a query that defines params, you can pass `--arg` to them as well:
```bash
powerpipe query run list_vpcs \
  --snapshot \
  --arg region='["us-east-1","us-east-2"]' \
  --arg account_id='["123412341234", "111111111111"]'
```

## Tagging Snapshots

You may want to tag your snapshots to make it easier to organize them.  You can use the `--snapshot-tag` argument to add a tag:

```bash
powerpipe dashboard run aws_insights.dashboard.aws_account_report \
  --snapshot \
  --snapshot-tag env=local 
```

Simply repeat the flag to add more than one tag:
```bash
powerpipe dashboard run aws_insights.dashboard.aws_account_report \
  --snapshot \
  --snapshot-tag env=local \
  --snapshot-tag owner=george 
```


## Saving Snapshots to Local Files

Turbot Pipes makes it easy to save and share your snapshots but it is not strictly required;  You can save and view snapshots using only the CLI.  

You can specify a local path in the `--snapshot-location` argument or `POWERPIPE_SNAPSHOT_LOCATION` environment variable to save your snapshots to a directory in your filesystem:

```bash
powerpipe benchmark run benchmark.cis_v150 --snapshot --snapshot-location . 
```

You can also set `snapshot_location` in a [workspace](/docs/run/workspaces) if you wish to make it the default location.


Alternatively, you can use the `--export` argument to export a query, control, dashboard, or benchmark in the Powerpipe snapshot format.  This will create a file with a `.pps` extension in the current directory:

```bash
powerpipe dashboard run dashboard.aws_account_report --export pps
```

The `snapshot` export/output type is an alias for `pps`:

```bash
powerpipe dashboard run dashboard.aws_account_report --export snapshot
```

To give the file a name, simply use `{filename}.pps`, for example:

```bash
powerpipe dashboard run dashboard.aws_account_report --export account_report.pps
```

Alternatively, you can write the Powerpipe snapshot to stdout with `--output pps`
```bash
powerpipe query run "select * from aws_account" --output pps > mysnap.pps
```

or `--output snapshot`
```bash
powerpipe query run  "select * from aws_account" --output snapshot  > mysnap.pps
```


## Controlling Output
When using `--share` or `--snapshot`, the output will include the URL to view the snapshot that you created in addition to the usual output:
```bash
Snapshot uploaded to https://pipes.turbot.com/user/costanza/workspace/vandelay/snapshot/snap_abcdefghij0123456789_asdfghjklqwertyuiopzxcvbn
```

You can use the `--progress=false` argument to suppress displaying the URL and other progress data.  This may be desirable when you are using an alternate output format, especially when piping the output to another command:

```bash
powerpipe query run "select * from aws_account" \
  --snapshot \
  --output json  \
  --progress=false  | jq
```

You can use all the usual `--export` or `--output` formats with `--snapshot` and `--share`.  Neither the `--output` nor the `--`export` flag affects the snapshot format though; the snapshot itself is always a JSON file that is saved to Turbot Pipes and viewable as HTML:

```bash
powerpipe benchmark run cis_v140 --snapshot --export cis.csv --export cis.json 
```

In fact, all the usual arguments will work with snapshots:
```bash
powerpipe control run aws_compliance.control.cis_v140_1_1 --snapshot  
powerpipe benchmark run aws_compliance.benchmark.cis_v140_ --snapshot --where "severity in ('critical', 'high')" all
```