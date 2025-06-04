---
title: Detection
sidebar_label: detection
---

# detection

**Detections** provide a defined structure for queries against logs [collected by Tailpipe](https://tailpipe.io/docs). These queries look for patterns and anomalies in logs.

## Example Usage

```hcl
detection "repository_visibility_set_public" {
  title           = "Repository Visibility Set Public"
  description     = "Detect when a private repository's visibility was set to public, potentially exposing proprietary or sensitive code."
  documentation   = file("./detections/docs/repository_visibility_set_public.md")
  severity        = "high"
  query           = query.repository_visibility_set_public
  display_columns = local.detection_display_columns_repository

  tags = merge(local.repository_common_tags, {
    mitre_attack_ids = "TA0001:T1195.002"
  })
}

query "repository_visibility_set_public" {
  sql = <<-EOQ
    select
      ${local.detection_sql_resource_column_repository}
    from
      github_audit_log
    where
      action = 'repo.access'
      and (additional_fields ->> 'visibility') = 'public'
      and (additional_fields ->> 'previous_visibility') = 'private'
    order by
      tp_timestamp desc;
  EOQ
}
```

You can run a detection from the command line:

```bash
powerpipe detection run repository_visibility_set_public
```

Detections can be organized into [benchmarks](https://powerpipe.io/docs/powerpipe-hcl/benchmark). You can run all detections for a benchmark:

```bash
powerpipe benchmark run audit_log_detections
```

## Argument Reference
| Argument | Type | Optional? | Description
|-|-|-|-
| `args` | Map | Optional| A map of arguments to pass to the query. The `args` argument may only be specified for detections that specify the `query` argument. 
| `database` | String |  Optional| A database [connection reference](/docs/reference/config-files/connection), [connection string](/docs/powerpipe-hcl/query#connection-strings), or [Pipes workspace](/docs/run/workspaces#implicit-workspaces) to query.  If not specified, the [default database](/docs/run#selecting-a-database ) will be used.
| `description` | String| Optional| A description of the detection.
| `display_columns` | List(String) | Optional | A list of columns to show by default.
| `documentation` | String (Markdown)| Optional | A markdown string containing a long form description, used as documentation for the mod on hub.powerpipe.io. 
| `param` | Block | Optional| A [param](/docs/powerpipe-hcl/query#param) block that defines the parameters that can be passed in to the detection's query.  `param` blocks may only be specified for detections that specify the `sql` argument. 
| `query` | Query Reference | Optional | A reference to a [query](/docs/powerpipe-hcl/query) resource that defines the detection query to run. A detection must either specify the `query` argument or the `sql` argument, but not both.
| `sql` | String | Required | An SQL string that returns rows found by the detection's query. A detection must either specify the `query` argument or the `sql` argument, but not both.
| `tags` | Map | Optional | A map of key:value metadata for the benchmark, used to categorize, search, and filter.  The structure is up to the mod author and varies by benchmark and provider. 
| `title` | String | Optional | Display title for the detection.
