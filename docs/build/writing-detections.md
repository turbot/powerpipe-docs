---
title: Writing Detections
---

# Writing Detections

Many detections and benchmarks are available in [mods on the Tailpipe Hub](https://hub.tailpipe.io/). However, if these don't meet your needs, Tailpipe makes it easy to create your own **detections** and **benchmarks** to tailor solutions to *your* organization. These can reflect your specific monitoring and security requirements while integrating seamlessly with your dashboards.

This guide introduces the core concepts for creating detections.

---

## What are Detections?

Detections in Tailpipe serve as queries that analyze logs or other data sources to identify patterns, anomalies, or issues of interest. Detections return **all relevant columns**, providing a complete context for interpretation.

---

## Example Detection

Letâ€™s build a simple detection for monitoring AWS CloudTrail logs.

### Prerequisites

To follow this example, ensure you have:

1. [Download and install Tailpipe](https://tailpipe.io/downloads).
2. Access to the CloudTrail logs you want to analyze, either locally or via a cloud service.
3. An active Tailpipe instance with your logs configured as a data source.

---

### Create a Detection

1. **Create a Mod**  
   Tailpipe resources are packaged into mods. First, [create a mod](https://docs.tailpipe.io/create-mod) to house your detection and benchmarks.

2. **Write a Detection Query**  
   Create a new file in your mod folder called `unauthorized_access.pp` and add the following code:

   ```hcl
   detection "unauthorized_access" {
     title = "Unauthorized Access Attempts"

     sql = <<EOT
       select
         event_time,
         event_source,
         event_name,
         user_identity,
         aws_region,
         source_ip_address,
         error_code,
         request_parameters
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

   - **Purpose**: This detection identifies unauthorized access attempts by querying `cloudtrail_logs` for events with error codes containing "Unauthorized."
   - **Query Output**: All columns from `cloudtrail_logs` are returned, offering a complete view of the event, including user identity, source IP, and the full error code.

3. **Run Your Detection**  
   Use the following command to run the detection:

   ```bash
   tailpipe detection run unauthorized_access
   ```

   This outputs the raw query results, which can be piped into a dashboard or further filtered as needed.

---

### Organize Detections in a Benchmark

Benchmarks allow grouping detections logically. For example, letâ€™s add another detection and create a benchmark:

```hcl
detection "suspicious_ip" {
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

benchmark "security_monitoring" {
  title = "Security Monitoring"
  children = [
    detection.unauthorized_access,
    detection.suspicious_ip,
  ]
}
```

Run the benchmark to execute all child detections:

```bash
tailpipe benchmark run security_monitoring
```

---

### Detections in Dashboards

The unfiltered results from detections make them ideal for dashboards. Users can apply filters dynamically, isolating relevant columns or drilling down into specific events.

---

This guide is a starting point. Explore the [Tailpipe Hub](https://hub.tailpipe.io/) for more examples and best practices to maximize your use of detections and benchmarks.