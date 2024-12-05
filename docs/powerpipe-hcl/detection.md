---
title: Detection
sidebar_label: detection
---

# detection

**Detections** provide a defined structure and interface for queries against logs [collected by Tailpipe](). These queries look for patterns and anomalies in logs.

## Example Usage

```hcl
detection "audit_logs_detect_failed_workflow_actions" {
  title           = "Detect Failed GitHub Actions"
  description     = "Detect instances in audit logs where GitHub Actions workflows fail, potentially indicating unauthorized changes, misconfigurations, or compromised workflows."
  severity        = "high"
  query           = query.audit_logs_detect_failed_workflow_actions
}

query "audit_logs_detect_failed_workflow_actions" {
  sql = <<-EOQ
    select
      ${local.audit_logs_detect_failed_workflow_actions_sql_columns}
    from
      github_audit_log
    where
      action = 'workflows.completed_workflow_run'
    order by
      tp_timestamp desc;
  EOQ
}
```

You can run a detection from the command line:

```bash
tailpipe detection run audit_logs_detect_failed_workflow_actions
```

Detections can be organized into [benchmarks](https://powerpipe.io/docs/powerpipe-hcl/benchmark). benchmarks. You can run all detections for a benchmark

```bash
tailpipe benchmark run audit_logs
```

## Argument Reference
| Argument | Type | Optional? | Description
|-|-|-|-
| `args` | Map | Optional| A map of arguments to pass to the query. The `args` argument may only be specified for controls that specify the `query` argument. 
| `database` | String |  Optional| A database [connection reference](/docs/reference/config-files/connection), [connection string](/docs/powerpipe-hcl/query#connection-strings), or [Pipes workspace](/docs/run/workspaces#implicit-workspaces) to query.  If not specified, the [default database](/docs/run#selecting-a-database ) will be used.
| `description` | String| Optional| A description of the control.
| `documentation` | String (Markdown)| Optional | A markdown string containing a long form description, used as documentation for the mod on hub.powerpipe.io. 
| `param` | Block | Optional| A [param](/docs/powerpipe-hcl/query#param) block that defines the parameters that can be passed in to the control's query.  `param` blocks may only be specified for controls that specify the `sql` argument. 
| `query` | Query Reference | Optional | A reference to a [query](/docs/powerpipe-hcl/query) resource that defines the control query to run.  The referenced query must conform to the control interface - it must return the [required control columns](#required-control-columns).  A control must either specify the `query` argument or the `sql` argument, but not both.
| `--search-path` | String |  Optional| Set a comma-separated list of connections to use as a custom search path for the query
| `--search-path-prefix` | String |  Optional| Set a comma-separated list of connections to use as a prefix to the current search path for the query.
| `sql` | String | Required | An SQL string that returns rows that conform to the control interface - it must return the [required control columns](#required-control-columns).  A control must either specify the `query` argument or the `sql` argument, but not both.
| `tags` | Map | Optional | A map of key:value metadata for the benchmark, used to categorize, search, and filter.  The structure is up to the mod author and varies by benchmark and provider. 
| `title` | String | Optional | Display title for the control.
