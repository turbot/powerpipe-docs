---
title: Using AI
sidebar_label: Using AI
---

# Using AI

Creating new benchmarks, dashboards, and controls for Powerpipe mods with AI tools and IDEs works remarkably well. At Turbot, we develop these components frequently and use AI for almost every new implementation. We've experimented with various approaches, including detailed prompt engineering, explicit guidelines, IDE rules and instructions, and complex workflows, but found that AI typically produces excellent results even without heavy guidance.

The key to this success is working within existing mod repositories and opening the entire repository as a folder or project in your IDE. This gives AI tools access to existing implementations, documentation examples, code patterns, and naming conventions to generate consistent, high-quality results without extensive prompting.

If you're looking to use AI to run Powerpipe controls rather than develop new ones, you can use the [Powerpipe MCP server](../run/mcp), which provides powerful tools for AI agents to inspect and run controls, benchmarks, and queries.

## Getting Started

While AI often works well with simple requests like "Create a control for [requirement]", here are some prompts we use at Turbot that you may find helpful as starting points.

### Prerequisites

1. Open the mod repository in your IDE (Cursor, VS Code, Windsurf, etc.) to give AI tools access to all existing code and documentation.
2. Ensure you have Powerpipe installed with all necessary plugins configured.
3. Set up access to the cloud providers or systems you'll be evaluating.
4. Configure the [Powerpipe MCP server](https://github.com/turbot/powerpipe-mcp) which allows the agent to inspect and run controls.

### Create Control

First, create the complete control implementation:

```md
Your goal is to create a new control with all necessary components.

1. Review existing controls and their documentation in the mod to understand established patterns, naming conventions, and query structures.

2. Create the control definition:
   - Choose appropriate directory and file based on existing patterns
   - Use descriptive names following conventions
   - Include clear descriptions and documentation
   - Set appropriate severity levels
   - Define any necessary variables with default values
   - Implement the SQL query following established patterns:
     - Use `->` and `->>` operators with spaces before and after
     - Include resource identifiers in non-aggregate queries
   - Add to relevant benchmarks (e.g., all controls benchmark, service-specific benchmark)
   - Include comprehensive documentation:
     - Clear description of what the control checks
     - Explanation of why this check is important
     - List of variables and their purposes
     - Example queries and expected results
     - Remediation steps for failing resources
     - Any limitations or important notes
```

### Run Control

Next, verify the control is properly configured:

```md
Your goal is to verify the control runs without errors.

1. If using the Powerpipe MCP server (preferred):
   - Set the mod location to the current directory
   - Use powerpipe_control_run to execute the control
   - Verify the control exists and can be queried

2. If not using MCP server:
   - Run `powerpipe control run <control_name>`
   - Check for any syntax or configuration errors

3. Verify:
   - Control executes without errors
   - Query syntax is valid
   - Variables are properly defined
   - Documentation examples are correct
```

### Create Test Resources

Set up resources to validate control behavior:

```md
Your goal is to create test resources that will trigger both passing and failing results.

1. Create test resources:
   - Use provider's CLI if available
   - Use Terraform if CLI isn't available
   - Use API calls via shell script as a last resort
   - Create resources that should:
     - Pass the control requirements
     - Fail the control requirements
     - Test edge cases
   - Use cost-effective configurations
   - Create any dependent resources needed

2. Document the test resources:
   - Resource identifiers
   - Expected control results
   - Steps to recreate
   - Cleanup instructions
```

### Validate Control Results

Verify the control produces correct results:

```md
Your goal is to validate the control produces expected results for test resources.

1. Run the control:
   - Use Powerpipe MCP server's powerpipe_control_run tool (preferred)
   - Or use Powerpipe CLI: powerpipe control run <control_name>

2. Verify:
   - Passing resources are correctly identified
   - Failing resources are correctly identified
   - Edge cases are handled properly
   - Result messages are clear and actionable
   - All example queries from documentation execute successfully
   - Results match example descriptions

3. Document all test results in raw Markdown format for easy review
```

### Cleanup Test Resources

Finally, clean up your test environment:

```md
Your goal is to remove all test resources to avoid ongoing costs.

1. Delete all resources created for testing:
   - Use the same method used for creation
   - Remove any dependent resources
   - Follow provider's recommended deletion order

2. Verify cleanup:
   - Confirm all resources are successfully deleted
   - Run the control again to verify 0 results or expected baseline
```

## Powerpipe HCL

For detailed information about writing Powerpipe HCL configurations, including benchmarks, controls, dashboards, and more, see the [Powerpipe HCL documentation](../develop/hcl).

Remember that AI tools work best when they can see the full context of your mod repository. Always open the entire repository in your IDE and use the Powerpipe MCP server when available for the most efficient development experience. 