---
title: Writing Dashboards
sidebar_label: Writing Dashboards
---

# Writing Dashboards

When creating a new dashboard, we have included some prompts below that you can use with your AI tools.

## Create Dashboard

For all dashboards, include the following prompt:

```
---
# Specify the following for Cursor rules
description: Guidelines for writing Powerpipe dashboards
alwaysApply: false
---

# Writing Powerpipe Dashboards

You are an expert in Powerpipe, Steampipe, and SQL. Use the following guidelines when creating Powerpipe dashboards.

## General

- Each dashboard must be defined in a `.pp` file in the appropriate mod directory.
- Dashboards should be grouped by service or use case for clarity and navigation.
- Use clear, descriptive titles and detailed documentation for each dashboard.
- Reference the [Powerpipe HCL dashboard documentation](/docs/powerpipe-hcl/dashboard) for all available arguments and options.

## Dashboard Structure

- Dashboards are defined using the `dashboard` block:
  - `title` - Short, descriptive name for the dashboard.
  - `documentation` (optional) - Path to a markdown file with extended documentation.
  - `tags` - Key-value metadata for filtering, reporting, and organization.
  - `container` - Use containers to group related cards, charts, tables, and other containers.
  - `input` (optional) - Define user inputs for dynamic dashboards.

- Example:

dashboard "ec2_instance_dashboard" {
  title         = "AWS EC2 Instance Dashboard"
  documentation = file("./dashboards/ec2/docs/ec2_instance_dashboard.md")
  tags = merge(local.ec2_common_tags, {
    type = "Dashboard"
  })

  container {
    card { query = query.ec2_instance_count width = 2 }
    card { query = query.ec2_instance_total_cores width = 2 }
    # ... more cards ...
  }
  # ... more containers ...
}

## Implementation Patterns

- Use containers to organize dashboard content by theme (e.g., Analysis, Costs, Performance).
- Use cards for key metrics and summary values.
- Use charts for visualizing trends, breakdowns, and comparisons.
- Use tables for detailed data views.
- Use inputs for user-driven filtering and interactivity.
- Use `width` to control layout and responsiveness (dashboard grid is 12 units wide).
- Use `href` in cards, tables, and charts to link to related dashboards or details.
- Use `series` and `point` blocks in charts to customize colors and labels.

## Common Elements

### Container
- Groups related dashboard elements.
- Can be nested for complex layouts.
- Supports `title` and `width`.

### Card
- Displays a single value, metric, or status.
- Supports `type` (plain, alert, info, ok), `icon`, `href`, and `width`.
- Use for KPIs, counts, and quick stats.

### Chart
- Visualizes data as bar, column, donut, line, pie, or heatmap.
- Supports `type`, `title`, `width`, `series`, `point`, and `legend`.
- Use for trends, breakdowns, and comparisons.

### Table
- Displays tabular data.
- Supports `title`, `width`, `column` options, and `href` for links.
- Use for detailed lists and drilldowns.

### Input
- Adds user input controls (select, multiselect, combo, text, etc.).
- Use `self.input.<name>.value` to reference input values in queries.
- Supports dynamic and static options.

## Tags

- Use standard tags for filtering and reporting:
  - `service` (e.g., `AWS/EC2`)
  - `type` (e.g., `Dashboard`)
  - Any framework or compliance tags as needed.
- Use `merge(local.<common_tags>, { ... })` to combine common and dashboard-specific tags.
```

## Documentation

```
---
# Specify the following for Cursor rules
description: Guidelines for writing Powerpipe dashboard documentation
alwaysApply: false
---

- Each dashboard should have a corresponding markdown file in the `docs/dashboards/` directory if extended documentation is needed.
- Use clear, concise language and provide context for the dashboard's purpose.
- Include example screenshots and usage notes where possible.
```

## Testing

```
---
# Specify the following for Cursor rules
description: Guidelines for testing Powerpipe dashboards
alwaysApply: false
---

# Testing Powerpipe Dashboards

You are an expert in Powerpipe and SQL. Use the following steps when testing dashboards:

## Create Resources
- Use the provider's CLI or API to create resources that will be visualized in the dashboard.
- Populate as many properties as possible to ensure all dashboard elements are tested.

## Run Dashboards
- You MUST use the Powerpipe MCP server to run the dashboard.
- Verify:
  - All cards, charts, tables, inputs, and other dashboard elements run correctly.
  - Data is accurate and up to date.
- Test all input-driven scenarios and edge cases.
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
