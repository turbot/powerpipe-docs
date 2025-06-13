---
title: Using AI
sidebar_label: Using AI
---

# Using AI

Creating new controls for Powerpipe mods with AI tools and IDEs works remarkably well. At Turbot, we develop controls frequently and use AI for almost every new control we create. We've found that AI typically produces excellent results when working with existing mod repositories, as it can learn from established patterns and conventions.

The key to success is working within existing mod repositories and opening the entire repository as a folder or project in your IDE. This gives AI tools access to existing control implementations, documentation examples, code patterns, and naming conventions to generate consistent, high-quality results.

If you're looking to use AI to run Powerpipe controls rather than develop new ones, you can use the [Powerpipe MCP server](../query/mcp), which provides powerful tools for AI agents to inspect and run controls, benchmarks, and queries.

## Getting Started

While AI often works well with simple requests like "Create a control for [requirement]", here are some prompts we use at Turbot that you may find helpful as starting points.

### Prerequisites

1. Open the mod repository in your IDE (Cursor, VS Code, Windsurf, etc.) to give AI tools access to all existing code and documentation.
2. Ensure you have Powerpipe installed with all necessary plugins configured.
3. Set up access to the cloud providers or systems you'll be evaluating.
4. Configure the [Powerpipe MCP server](https://github.com/turbot/powerpipe-mcp) which allows the agent to inspect and run controls.

### Understand Conventions

First, review existing controls and conventions to ensure consistency:

```md
Your goal is to understand the conventions for creating a new Powerpipe control.

1. Review the sample repository at https://github.com/turbot/steampipe-mod-aws-thrifty for reference patterns.

2. Analyze existing controls in the current mod to understand:
   - Control naming conventions
   - File and directory organization
   - Query structure and style
   - Variable naming and placement
   - Documentation format and requirements

3. Document the key conventions you've identified to ensure your new control will be consistent.
```

### Create and Test the Query

Next, develop the SQL query that will power your control:

```md
Your goal is to create a working SQL query for your new control.

1. Follow the query structure conventions identified earlier.

2. Test the query using one of these methods:
   - Powerpipe MCP server's steampipe_query tool (preferred)
   - Steampipe CLI query command

3. Verify the query:
   - Returns expected results
   - Handles edge cases appropriately
   - Follows performance best practices
   - Uses consistent operators (e.g., '->' and '->>' with spaces)

4. Do not proceed until you have a fully working and tested query.
```

### Create the Control

With a working query, create the control definition:

```md
Your goal is to implement the control using your tested query.

1. Choose the appropriate directory and file based on conventions.

2. Create the control definition:
   - Use descriptive names following conventions
   - Include clear descriptions and documentation
   - Set appropriate severity levels
   - Define any necessary variables
   - Reference your tested query

3. Handle variables properly:
   - Place non-shared variables in the control file
   - Only use variables.pp for shared variables
   - Set appropriate default values
```

### Add Documentation

Document your new control thoroughly:

```md
Your goal is to create comprehensive documentation for your control.

1. Choose the appropriate documentation location based on conventions.

2. Include:
   - Clear description of what the control checks
   - Explanation of why this check is important
   - List of variables and their purposes
   - Example queries and expected results
   - Remediation steps for failing resources
   - Any limitations or important notes

3. Follow existing documentation format and style.
```

### Test the Control

Finally, test your complete control implementation:

```md
Your goal is to verify your control works correctly end-to-end.

1. Test using one of these methods:
   - Powerpipe MCP server's powerpipe_control_run tool (preferred)
     - First set mod-location to current directory
   - Powerpipe CLI command: powerpipe control run <control_name>

2. Verify:
   - Control executes without errors
   - Results match expectations
   - Documentation examples work correctly
   - Variables function as intended

3. Make any necessary adjustments and retest until everything works perfectly.
```

## Best Practices

1. **Start Small**: Begin with simple controls and gradually add complexity as needed.
2. **Test Thoroughly**: Always test your query independently before implementing the control.
3. **Follow Patterns**: Stick to established conventions from existing controls.
4. **Document Clearly**: Write documentation that helps users understand both what the control does and why it's important.
5. **Consider Performance**: Optimize queries for large-scale environments.
6. **Use Variables**: Make controls flexible with variables, but keep defaults sensible.
7. **Think Modular**: Design controls that can be reused across different benchmarks.

## Common Pitfalls to Avoid

1. **Overly Complex Queries**: Keep queries focused and simple when possible.
2. **Missing Edge Cases**: Test with various resource configurations.
3. **Hardcoded Values**: Use variables for configurable thresholds.
4. **Incomplete Documentation**: Always include examples and remediation steps.
5. **Inconsistent Naming**: Follow established naming patterns.
6. **Poor Error Handling**: Consider and handle potential error conditions.

Remember that AI tools work best when they can see the full context of your mod repository. Always open the entire repository in your IDE and use the Powerpipe MCP server when available for the most efficient development experience. 