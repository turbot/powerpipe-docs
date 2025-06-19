---
title: Using AI
sidebar_label: Using AI
---

# Using AI

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

First, create the new control and its documentation, using existing controls as reference for patterns and conventions.

```md
Your goal is to create a new control for <resource type> and <check condition> with all necessary components.

1. Review existing controls to understand mod patterns and conventions:
   - Directory structure and file placement
   - Query patterns and common table joins
   - Benchmark organization

2. Create the control definition with:
   - Descriptive name that reflects the requirement
   - Required variables with meaningful defaults
   - SQL query following plugin-specific patterns
   - Assignments to service and category benchmarks

3. Add documentation as per the structure of the other controls in the mod.
```

### Run Control

Next, verify your control is properly configured and can be executed without errors.

```md
Your goal is to verify the control runs without errors.

1. Using MCP server (preferred):
   - Set mod-location to your repository
   - Run control and check for errors

2. Using CLI:
   - Run 'powerpipe control run <control_name>'
   - Check for syntax or configuration errors
```

### Create Test Resources

To test the control's functionality, you'll need resources that will trigger both passing and failing results.

```md
Your goal is to create test resources for validation.

1. Create resources using provider's CLI, Terraform, or API that are:
- Compliant with all requirements
- Non-compliant with common misconfigurations
- Testing edge cases and boundary conditions

2. Verify that all resources were created successfully using the same tool or method used for creation.
```

### Validate Results

Now validate that your control correctly identifies compliant and non-compliant resources.

```md
Your goal is to validate control behavior.

1. Run control using MCP server or CLI.

2. Verify:
   - Compliant resources pass with clear reason
   - Non-compliant resources fail with actionable message
   - Edge cases produce expected results
   - Query performance meets expectations
```

### Cleanup Test Resources

Finally, remove all test resources to avoid ongoing costs and maintain a clean test environment.

```md
Your goal is to remove test resources.

1. Delete resources following provider dependencies:
   - Remove child resources first
   - Handle linked resource cleanup
   - Verify no cost-incurring resources remain

2. Validate cleanup:
   - Run control to confirm zero results
   - Check provider console/CLI for remnants
```

## Powerpipe HCL

For detailed information about writing Powerpipe HCL configurations, see the [Powerpipe HCL documentation](../develop/hcl).

Remember to keep the entire repository open in your IDE to give AI tools full context of your mod. 