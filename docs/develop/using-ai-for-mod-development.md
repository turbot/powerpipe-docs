---
title: Using AI for Mod Development
sidebar_label: Using AI for Mod Development
---

# Using AI for Mod Development

Creating new benchmarks, dashboards, and controls for Powerpipe mods with AI tools and IDEs works remarkably well. At Turbot, we develop these components frequently and use AI for almost every new implementation. The key is working within existing mod repositories - this gives AI tools access to established patterns and conventions to generate consistent, high-quality results.

If you're looking to use AI to run Powerpipe benchmarks, dashboards, and controls rather than develop new ones, you can use the [Powerpipe MCP server](../run/mcp), which provides powerful tools for AI agents to inspect and run controls, benchmarks, and queries.

## Getting Started

While AI often works well with simple requests like "Create a control for [requirement]", here are some prompts we use at Turbot that you may find helpful as starting points.

### Prerequisites

1. Open the mod repository in your IDE (Cursor, VS Code, Windsurf, etc.) to give AI tools access to all existing code and documentation.
2. Ensure you have Powerpipe installed with all necessary plugins configured.
3. Set up access to create test resources in the provider.
4. Configure the [Powerpipe MCP server](https://github.com/turbot/powerpipe-mcp) which allows the agent to run controls.

### Create Control

First, create the complete control and its documentation, using existing controls and documentation as reference.

#### Prompt

```md
Your goal is to create a new Powerpipe control for <resource type> to check for <condition>.

1. Review existing benchmarks and controls and their documentation in the mod to understand the established patterns, naming conventions, and query structure.

2. Create the control and add it to any benchmark with similar controls in it.
```

### Run Control

Next, verify your control is properly configured and can be executed without errors.

#### Prompt

```md
Your goal is to verify the <resource type> control compiles and runs correctly.

1. Check the Steampipe service status with `steampipe service status`. Start it with `steampipe service start` if not running, or restart it with `steampipe service restart` if already running.

2. Test if the Powerpipe MCP server is available by running the `powerpipe_mod_location` tool.

3. If the MCP server is available, use it to verify the control exists and can be run successfully.
  - When passing in the control name, do not add a qualifier, just pass the control name

4. If the MCP server is not available, verify the control exists with `powerpipe control list` and can be run with `powerpipe control run <control_name>`.
```

### Create Test Resources

To test the control's functionality, you'll need resources to query. You can either use existing resources or create new test resources with appropriate properties.

#### Prompt

```md
Your goal is to create test resources for <resource_type> to check for <condition> to validate your Powerpipe control implementation.

1. Create test resources with as many properties set as possible.
  - Use the provider's CLI if available, Terraform configuration if CLI isn't available, or API calls via shell script as a last resort.
  - Create any dependent resources needed.
  - Use the most cost-effective configuration. If the estimated cost is high, e.g., $50, warn about the expense rather than proceeding.

2. Verify that all resources were created successfully using the same tool or method used for creation.
```

### Validate Results

Next, run the control to verify it correctly identifies and evaluates resources.

#### Prompt

```md
Your goal is to verify the control correctly evaluates the test resources.

Use the Powerpipe MCP server for running controls if available, otherwise use the `powerpipe` CLI commands directly.

1. Run the control against your test resources.

2. Verify the control returns the expected state for each resource.

3. Confirm the control's reason field accurately describes the current state of the resource.

4. Share the validation results in raw Markdown format, including the full control output showing resource evaluation.
```

### Cleanup Test Resources

After testing is completed, remove any resources created for testing.

#### Prompt

```md
Your goal is to clean up all test resources created for <resource type> validation to avoid ongoing costs.

1. Delete all resources created for testing, including any dependent resources, using the same method that was used to create them.

2. Verify that all resources were successfully deleted, using the same method that was used to delete them.
```
