---
title: benchmark
---


# benchmark

The benchmark block provides a mechanism for grouping controls or detections into benchmarks, and into sections within a  benchmark.  For instance, the Powerpipe AWS mod has separate top-level benchmarks for CIS, NIST, PCI, etc.  Each of these benchmarks may have sub-level benchmarks to organize controls in a way that reflects that particular control framework — one for each section or subsection in CIS, one for each CSF Core Function and subcategory for NIST, etc. The Tailpipe AWS mod works similarly for detections.

A `benchmark` may specify `control` or `detection` or `benchmark` resources as children, enabling you to create flexible hierarchies of any depth.  By default, controls will be grouped with aggregated totals at each benchmark. [[Not true for detections, right?]]

You can run benchmarks or refer to them with HCL syntax as `{mod}.benchmark.{name}`.  The name must be unique in the namespace 
(mod). Typically, controls, detection, and benchmarks in a given benchmark should be named in a way that mimics the hierarchy in order to provide an easy-to-follow structure.  This is a convention that should be followed, but not a strict requirement.  

You can run controls, detections, and benchmarks with the [powerpipe benchmark run](/docs/reference/cli/benchmark#powerpipe-benchmark-run) and [powerpipe control run](/docs/reference/cli/control#powerpipe-control-run) commands.

You can [view benchmarks as dashboards](/docs/run/dashboard) with the [powerpipe server](/docs/reference/cli/server) command.

And you can run individual controls or detections with `powerpipe control run` or `powerpipe detection run`.

## Example Usage

```hcl
benchmark "cisv130" {
  title         = "CIS v1.3.0"
  description   = "The CIS Amazon Web Services Foundations Benchmark provides prescriptive guidance for configuring security options for a subset of Amazon Web Services with an emphasis on foundational, testable, and architecture agnostic settings."
  documentation = file("./docs/cis-overview.md")

  children = [
    benchmark.cis_v130_1,
    benchmark.cis_v130_2,
    benchmark.cis_v130_3,
    benchmark.cis_v130_4,
    benchmark.cis_v130_5
  ]

  tags = {
    cloud_provider = "aws" 
    framework      = "cis"
    cis_version    = "v1.3.0"
  }
}

benchmark "cisv130_1" {
  title         = "1 Identity and Access Management"
  documentation = file("./docs/cisv130_1.md")

  children = [
    control.cis_v130_1_1,
    control.cis_v130_1_2,
    control.cis_v130_1_3,
    control.cis_v130_1_4,
    control.cis_v130_1_5,
    control.cis_v130_1_6
  ]

  tags = {
    cloud_provider = "aws" 
    framework      = "cis"
    cis_version    = "v1.3.0"
  }
}

control "cisv130_1_4" {
  title         = "1.4 Ensure no root user account access key exists (Automated)"
  description   = "The root user account is the most privileged user in an AWS account. AWS Access Keys provide programmatic access to a given AWS account. It is recommended that all access keys associated with the root user account be removed."
  sql           = query.iam_credential_report_root_user_access_key.sql
  documentation = file("./docs/cisv130_1_4.md")


  tags = {
    cloud_provider = "aws" 
    framework      = "cis"
    cis_version    = "v1.3.0"
    cis_item_id    = "1.4"
    cis_control    = "4.3"
    cis_type       = "automated"
    cis_level      = "1"
  }
}

```

## Argument Reference
| Argument |Type | Required? | Description
|-|-|-|-
| `children` | List |  Optional| An ordered list of `control` and/or `benchmark` references that are members (direct descendants) of the benchmark.
| `description` | String |  Optional| A description of the benchmark
| `documentation` | String (Markdown)| Optional | A markdown string containing a long form description, used as documentation for the mod on hub.powerpipe.io. 
| `tags` | Map | Optional | A map of key:value metadata for the benchmark, used to categorize, search, and filter.  The structure is up to the mod author and varies by benchmark and provider. 
| `title` | String | Optional | A display title for the benchmark
