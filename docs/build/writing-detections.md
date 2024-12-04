---
title: Writing Detections
---

# Writing Detections

Many detections and benchmarks are available in [mods on the Powerpipe Hub](https://hub.powerpipe.io/). However, if these don't meet your needs, Tailpipe makes it easy to create your own detections and benchmarks to tailor solutions to *your* organization.

This guide introduces the core concepts for creating detections and benchmarks

## What are Detections?

Detections in Tailpipe serve as queries that analyze logs or other data sources to identify patterns, anomalies, or issues of interest. Detections return all selected columns as context for filter-enabled analyis in Powerpipe.

## Example Detection

Let's build a simple detection for monitoring AWS CloudTrail logs, and wrap it in a benchmark.

### Prerequisites

1. [Download and install Tailpipe](https://tailpipe.io/downloads).
2. A download of the CloudTrail logs you want to analyze.
3. A `~/.tailpipe/config/aws.tpc` your logs configured as a data source. For example, assuming you have downloaded events to `~/tailpipe`

```
partition "cloudtrail" "cloudtrail_log" {
    source "file_system" {
        paths = ["~/tailpipe"]
        extensions = [".json"]
    }
}
```

### Create a Detection

1. **Create a Mod**  
   Tailpipe resources are packaged into mods. First, [create a mod](https://docs.tailpipe.io/create-mod) for benchmark and the detections it wraps.

2. **Define a detection**

Create a new file in your mod folder called `cloudtrail.pp` and add the following code:

```hcl
detection_benchmark "cloudtrail_log_detections" {
  title       = "Cloudtrail Log Detections"
  description = "This detection benchmark contains recommendations when scanning Cloudtrail logs."
  type        = "detection"
  children = [
    detection.cloudtrail_logs_detect_unauthorized_access,
  ]
  
detection "cloudtrail_logs_detect_unauthorized_access" {
  title = "Unauthorized Access Attempts"

  sql = <<EOT
    select
      *
    from
      cloudtrail_logs
    where
      error_code is not null
      and error_code like '%Unauthorized%'
    order by
      event_time desc
  EOT
}
```

To run the detection in the Powerpipe CLI:

```
powerpipe detection run cloudtrail_logs_detect_unauthorized_access
```

To view the mod:

```
powerpipe server
```

Then open localhost:9033 in a browser.

### Wrap the detection in a benchmark

The benchmark block enables you to [group detections](/docs/powerpipe-hcl/benchmark).


```hcl
benchmark "cloudtrail_log_detections"
  title       = "Cloudtrail Log Detections"
  description = "This detection benchmark contains recommendations when scanning Cloudtrail logs."
  type        = "detection"
  children = [
    detection.cloudtrail_logs_detect_unauthorized_access,
  ]
  
detection "cloudtrail_logs_detect_unauthorized_access" {
  title = "Unauthorized Access Attempts"

  sql = <<EOT
    select
      *
    from
      cloudtrail_logs
    where
      error_code is not null
      and error_code like '%Unauthorized%'
    order by
      event_time desc
  EOT
}
```

### Add a detection

Let's add another detection to the benchmark.

```hcl

detection_benchmark "cloudtrail_log_detections" {
  title       = "Cloudtrail Log Detections"
  description = "This detection benchmark contains recommendations when scanning Cloudtrail logs."
  type        = "detection"
  children = [
    detection.cloudtrail_logs_detect_unauthorized_access,
    detection.cloudtrail_logs_detect_suspicious_ips,

  ]

detection "cloudtrail_logs_detect_suspicious_ips" {
  title = "Suspicious IP Activity"
  sql = <<EOT
    select
      event_time,
      event_source,
      event_name,
      source_ip_address,
      user_identity,
      aws_region
    from
      cloudtrail_logs
    where
      source_ip_address in ('192.0.2.1', '203.0.113.5')
    order by
      event_time desc
    EOT
}
```


This guide is a starting point. Explore the [Powerpipe Hub](https://hub.powerpipe.io/) for more examples and best practices to maximize your use of detections and benchmarks.