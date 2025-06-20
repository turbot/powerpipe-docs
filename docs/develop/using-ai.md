---
title: Using AI for Control Development
sidebar_label: Using AI for Control Development
---

# Using AI for Control Development

Creating new benchmarks, dashboards, and controls for Powerpipe mods with AI tools and IDEs works remarkably well. At Turbot, we develop these components frequently and use AI for almost every new implementation. The key is working within existing mod repositories - this gives AI tools access to established patterns and conventions to generate consistent, high-quality results.

If you're looking to use AI to run Powerpipe controls rather than develop new ones, you can use the [Powerpipe MCP server](../run/mcp), which provides powerful tools for AI agents to inspect and run controls, benchmarks, and queries.

## Getting Started

While AI often works well with simple requests like "Create a control for [requirement]", here are some prompts we use at Turbot that you may find helpful as starting points.

### Prerequisites

1. Open the mod repository in your IDE (Cursor, VS Code, Windsurf, etc.) to give AI tools access to all existing code and documentation.
2. Ensure you have Powerpipe installed with all necessary plugins configured.
3. Set up access to create test resources in the provider.
4. Configure the [Powerpipe MCP server](https://github.com/turbot/powerpipe-mcp) which allows the agent to run controls.

### Create Control

First, create the new control using existing controls and docs as reference.

#### Prompt

```md
Your goal is to create a new Powerpipe control for <service> <resource type> and <check condition>.

1. Review existing benchmarks and controls and their documentation in the mod to understand the established patterns, naming conventions, and query structure.

2. Create the control and add it to any benchmark with similar controls in it.

```

### Run Control

Next, verify your control is properly configured and can be executed without errors.

#### Prompt

```md
Your goal is to verify the <service> <resource type> control compiles and runs correctly.

1. Check the Steampipe service status with `steampipe service status`. Start it with `steampipe service start` if not running, or restart it with `steampipe service restart` if already running.

2. Test if the Powerpipe MCP server is available by running the `powerpipe_mod_location` tool.

3. If the MCP server is available, use it to verify the control exists and can be run successfully.
  - When passing in the control name, do not add a qualifier, just pass the control name

4. If the MCP server is not available, verify the control exists with `powerpipe control list` and can be run with `powerpipe control run <control_name>`.
```

### Create Test Resources

To test the control's functionality, you'll need resources to query. You can either use existing resources or create new test resources with appropriate properties.

#### Prompt

To test the control's functionality, you'll need resources that will trigger both passing and failing results.

```md
Your goal is to create test resources for validation.

1. Create resources using provider's CLI, Terraform, or API.
   - Use the most cost-effective configuration. If the estimated cost is high, e.g., $50, warn about the expense rather than proceeding.

2. Verify that all resources were created successfully using the same tool or method used for creation.
```

### Validate Results

Next, run the control to verify it correctly identifies the test resources.

#### Prompt

```md
Your goal is to verify the control correctly identifies compliant and non-compliant resources.

1. Run the control using the MCP server or `powerpipe control run <control_name>`.

2. Verify the control returns expected results.

```

### Cleanup Test Resources

After testing is completed, remove any resources created for testing.

#### Prompt

```md
Your goal is to remove all test resources created for this control.

1. Delete all test resources using the same tool used to create them (CLI, Terraform, API).

2. Run the control again to verify no resources are returned.
```
