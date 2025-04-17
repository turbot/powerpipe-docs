---
title: AI Tools (MCP)
---

# AI Tools (MCP)

The [Powerpipe MCP server](https://github.com/turbot/powerpipe-mcp) (Model Control Protocol) transforms how you interact with your cloud infrastructure and compliance data.  It brings the power of conversational AI to your cloud resources, enabling natural language exploration and analysis of your cloud security posture:
- Exploration of security benchmarks and compliance frameworks
- Analysis of compliance status and findings
- Development and customization of controls
- Remediation guidance for failed checks

The MCP is packaged separately and runs as an integration in your AI tool, such as [Claude Desktop](https://claude.ai/download) or [Cursor](https://www.cursor.com/).

## Installation

### Prerequisites

- [Powerpipe](https://powerpipe.io/downloads) installed and configured
- [Node.js](https://nodejs.org/) v16 or higher (includes `npx`)
- An AI assistant that supports [MCP](https://modelcontextprotocol.io/introduction), such as [Cursor](https://www.cursor.com/) or Anthropic's [Claude Desktop](https://claude.ai/download).

### Configuration

The Powerpipe MCP server is packaged and distributed as an NPM package; just add Powerpipe MCP to your AI assistant's configuration file and restart your AI assistant for the changes to take effect:

```json
{
  "mcpServers": {
    "powerpipe": {
      "command": "npx",
      "args": [
        "-y",
        "@turbot/powerpipe-mcp",
        "/path/to/your/mod/is/required"
      ]
    }
  }
}
```
The mod location argument is required and must point to a directory containing your Powerpipe mod files. This is where Powerpipe will look for benchmarks, controls, and other resources.


| Assistant | Config File Location | Setup Guide |
|-----------|---------------------|-------------|
| Claude Desktop | `claude_desktop_config.json` | [Claude Desktop MCP Guide →](https://modelcontextprotocol.io/quickstart/user) |
| Cursor | `~/.cursor/mcp.json` | [Cursor MCP Guide →](https://docs.cursor.com/context/model-context-protocol) |

Refer to the [README](https://github.com/turbot/powerpipe-mcp/blob/main/README.md) for additional configuration options.


## Querying Powerpipe

To query Powerpipe, just ask questions using natural language!


Explore available compliance frameworks:
```
What Powerpipe benchmarks do we have available?
```

Simple, specific questions work well:
```
Show me all controls related to S3 bucket encryption in the CIS AWS benchmark
```

Generate a compliance report:
```
What's our current compliance status for the NIST controls?
```

Dive into the details:
```
Find all failed controls in the AWS Security benchmark and explain why they failed
```

Get information about specific requirements:
```
Show me all controls related to password policies across our benchmarks
```

Explore with wide-ranging questions:
```
Analyze our compliance gaps and suggest remediation steps
```


## Best Practices for Prompts

To get the most accurate and helpful responses from the MCP service, consider these best practices when formulating your prompts:

1. **Use natural language**: The LLM will handle the SQL translation
2. **Be specific**: Indicate which benchmarks or frameworks you're interested in
3. **Include context**: Mention the type of controls you want to analyze (encryption, access, networking, etc.)
4. **Iterative refinement**: Start with simple questions and then refine based on initial results
5. **Be bold and exploratory**:  It's amazing what the LLM will discover and achieve!

## Limitations
- Complex tasks may require iterative refinement.
- Response times depend on both the LLM API latency and query execution time.
- The MCP server only runs locally at this time.  You must run it from the same machine as your AI assistant.
- A valid subscription to the LLM provider is recommended; free plan limits are often insufficient for using the Powerpipe MCP server.