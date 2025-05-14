---
title: Writing Controls
sidebar_label: Writing Controls
---

# Writing Controls

When creating a new control, we have included some prompts below that you can use with your AI tools.

## Create Control

For all controls, include the following prompt:

```
---
# Specify the following for Cursor rules
description: Guidelines for writing Powerpipe controls
alwaysApply: false
---

# Writing Powerpipe Controls

You are an expert in Powerpipe, Steampipe, and SQL. Use the following guidelines when creating Powerpipe controls.

## General

- Each control must be defined in a `.sp` file in the appropriate mod directory.
- Controls should be grouped into logical benchmarks for organization and reporting.
- Use clear, descriptive titles and detailed descriptions for each control.
- Reference the [Powerpipe HCL control documentation](/docs/powerpipe-hcl/control) for all available arguments and required columns.

## Control Structure

- Controls are defined using the `control` block:
  - `title` - Short, descriptive name for the control.
  - `description` - Detailed explanation of what the control checks and why it matters.
  - `sql` or `query` - The SQL statement or query reference that implements the control logic.
  - `tags` - Key-value metadata for filtering, reporting, and organization.
  - `documentation` (optional) - Path to a markdown file with extended documentation.
  - `param` (optional) - Define parameters for dynamic controls (see below).

- Example:

```hcl
control "ec2_instance_in_vpc" {
  title       = "EC2 instances should be in a VPC"
  description = "Deploy EC2 instances within a VPC to enable secure communication between an instance and other services within the VPC, without requiring an internet gateway, NAT device, or VPN connection."

  sql = <<-EOQ
    select
      arn as resource,
      case
        when vpc_id is null then 'alarm'
        else 'ok'
      end as status,
      case
        when vpc_id is null then title || ' not in VPC.'
        else title || ' in VPC.'
      end as reason
      ${local.tag_dimensions_sql}
      ${local.common_dimensions_sql}
    from
      aws_ec2_instance;
  EOQ

  tags = merge(local.aws_perimeter_common_tags, {
    service = "AWS/EC2"
  })
}
```

## Implementation Patterns

- Controls should:
  - Use clear, deterministic SQL to evaluate compliance.
  - Return the required columns: `resource`, `status`, and `reason`.
  - Use additional dimensions (e.g., `account_id`, `region`) for context when available.
  - Use parameter blocks (`param`) for controls that require user input or variable values.
  - Use local variables for common SQL fragments (e.g., tag and dimension columns).
  - Implement logic for all relevant resource states (e.g., `ok`, `alarm`, `skip`, `info`).
  - Use `merge()` for tags to include common and control-specific tags.

## Required Columns

All controls MUST return the following columns:

- `resource` (string): Unique identifier for the resource (e.g., ARN, ID).
- `status` (string): One of `ok`, `alarm`, `skip`, `info`, or `error`.
- `reason` (string): Human-readable explanation for the status.

See [Powerpipe control documentation](/docs/powerpipe-hcl/control#required-control-columns) for details.

## Standard Tags

- Use standard tags for filtering and reporting:
  - `service` (e.g., `AWS/EC2`)
  - `type` (e.g., `Benchmark`, if part of a benchmark)
  - Any framework or compliance tags as needed (e.g., `cis_level`, `pci`, etc.)
- Use `merge(local.<common_tags>, { ... })` to combine common and control-specific tags.

## Parameters

- Use the `param` block to define parameters for controls that require user input.
- Reference variables using `var.<name>` for defaults.
- Example:

```hcl
control "vpc_peering_connection_cross_account_shared" {
  ...
  param "trusted_accounts" {
    description = "A list of trusted accounts."
    default     = var.trusted_accounts
  }
  ...
}
```
```

## Documentation

```
---
# Specify the following for Cursor rules
description: Guidelines for writing Powerpipe control documentation
alwaysApply: false
---

- Each control should have a corresponding markdown file in the `docs/controls/` directory if extended documentation is needed.
- Use clear, concise language and provide context for why the control is important.
- Include example queries and expected results where possible.
```

## Testing

```
---
# Specify the following for Cursor rules
description: Guidelines for testing Powerpipe controls
alwaysApply: false
---

# Testing Powerpipe Controls

You are an expert in Powerpipe and SQL. Use the following steps when testing controls:

## Build
- Build the mod locally and ensure all dependencies are installed.

## Create Resources
- Use the provider's CLI or API to create resources that will be evaluated by the control.
- Populate as many properties as possible to ensure all columns are tested.

## Start Powerpipe Service
- Start or restart the Powerpipe service as needed.

## Query
- Use the Powerpipe CLI or MCP server to run the control:
  - `powerpipe control run <control_name>`
- Verify:
  - All required columns are returned.
  - Status and reason values are correct for all test cases.
  - All edge cases (e.g., missing values, multiple accounts) are handled.
- Run all example queries in the control documentation.
```

## Cleanup

```
---
# Specify the following for Cursor rules
description: Guidelines for cleaning up testing resources
alwaysApply: false
---

# Clean Up Testing Resources

- All resources used for testing (including dependencies) MUST be deleted.
- Use the provider's CLI or API to delete resources after testing.
```