---
title: Writing Detections
sidebar_label: Writing Detections
---

# Writing Detections

When creating a new detection, we have included some prompts below that you can use with your AI tools.

## Create Detection

For all detections, include the following prompt:

```
---
# Specify the following for Cursor rules
description: Guidelines for writing Powerpipe detections
alwaysApply: false
---

# Writing Powerpipe Detections

You are an expert in Powerpipe, Tailpipe, and SQL. Use the following guidelines when creating Powerpipe detections.

## General

- Each detection must be defined in a `.pp` file in the appropriate mod directory.
- Detections should be grouped into logical benchmarks for organization and reporting.
- Use clear, descriptive titles and detailed descriptions for each detection.
- Reference the [Powerpipe HCL detection documentation](/docs/powerpipe-hcl/detection) for all available arguments and required columns.

## Detection Structure

- Detections are defined using the `detection` block:
  - `title` - Short, descriptive name for the detection.
  - `description` - Detailed explanation of what the detection checks and why it matters.
  - `severity` - The impact level (e.g., low, medium, high, critical).
  - `query` or `sql` - The query or SQL statement that implements the detection logic.
  - `tags` - Key-value metadata for filtering, reporting, and organization.
  - `documentation` (optional) - Path to a markdown file with extended documentation.
  - `display_columns` (optional) - Columns to display in the detection output.
  - `param` (optional) - Define parameters for dynamic detections (see below).

- Example:

```hcl
detection "ec2_ami_shared_publicly" {
  title           = "EC2 AMI Shared Publicly"
  description     = "Detect when an EC2 AMI was shared publicly. Sharing an AMI publicly may expose the image to unauthorized users, potentially leading to data leakage or security vulnerabilities."
  documentation   = file("./detections/docs/ec2_ami_shared_publicly.md")
  severity        = "medium"
  display_columns = local.detection_display_columns
  query           = query.ec2_ami_shared_publicly
  tags = merge(local.ec2_common_tags, {
    mitre_attack_ids = "TA0001:T1078.004"
  })
}
```

## Implementation Patterns

- Detections should:
  - Use clear, deterministic SQL to identify relevant log events or anomalies.
  - Return the required columns for detections (see below).
  - Use additional dimensions (e.g., `account_id`, `region`, `event_time`) for context when available.
  - Use parameter blocks (`param`) for detections that require user input or variable values.
  - Use local variables for common SQL fragments or tag sets.
  - Implement logic for all relevant event types and edge cases.
  - Use `merge()` for tags to include common and detection-specific tags.

## Required Columns

All detections MUST return the following columns:

- `resource` (string): Unique identifier for the resource or event (e.g., instance ID, image ID).
- Any additional columns required for context or reporting (e.g., `event_time`, `user_identity`).

See [Powerpipe detection documentation](/docs/powerpipe-hcl/detection#required-control-columns) for details.

## Tags

- Use standard tags for filtering and reporting:
  - `service` (e.g., `AWS/EC2`)
  - `type` (e.g., `Benchmark`, if part of a benchmark)
  - Any framework or compliance tags as needed (e.g., `mitre_attack_ids`).
- Use `merge(local.<common_tags>, { ... })` to combine common and detection-specific tags.

## Parameters

- Use the `param` block to define parameters for detections that require user input.
- Reference variables using `var.<name>` for defaults.
```


## Documentation

```
---
# Specify the following for Cursor rules
description: Guidelines for writing Powerpipe detection documentation
alwaysApply: false
---

- Each detection should have a corresponding markdown file in the `docs/detections/` directory if extended documentation is needed.
- Use clear, concise language and provide context for why the detection is important.
- Include example queries and expected results where possible.
```

## Testing

```
---
# Specify the following for Cursor rules
description: Guidelines for testing Powerpipe detections
alwaysApply: false
---

# Testing Powerpipe Detections

You are an expert in Powerpipe and SQL. Use the following steps when testing detections:

## Build
- Build the mod locally and ensure all dependencies are installed.

## Create Resources
- Use the provider's CLI or API to generate log events that will be detected.
- Populate as many properties as possible to ensure all detection logic is tested.

## Start Powerpipe Service
- Start or restart the Powerpipe service as needed.

## Run and Verify
- Use the Powerpipe CLI or MCP server to run the detection:
  - `powerpipe detection run <detection_name>`
- Verify:
  - All required columns are returned.
  - Detection logic correctly identifies relevant events.
  - All edge cases (e.g., missing values, multiple accounts) are handled.
- Run all example queries in the detection documentation.
```

## Cleanup

```
---
# Specify the following for Cursor rules
description: Guidelines for cleaning up testing resources
alwaysApply: false
---

# Clean Up Testing Resources

- All resources and log events used for testing (including dependencies) MUST be deleted or cleaned up.
- Use the provider's CLI or API to delete resources and clear logs after testing.
```