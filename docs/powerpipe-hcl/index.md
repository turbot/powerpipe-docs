---
title: Powerpipe HCL
---

# Powerpipe HCL

## General

| Type | Description
|-|-
| [locals](/docs/powerpipe-hcl/locals) | Locals are internal, module level variables.
| [mod](/docs/powerpipe-hcl/mod)     | The mod block contains metadata, documentation, and dependency data for the mod.
| [query](/docs/powerpipe-hcl/query) | Queries define common SQL statements that may be used alone, or referenced by arguments in other blocks like reports and actions.
| [variable](/docs/powerpipe-hcl/variable) | Variables are module level objects that essentially act as parameters for a module.


## Benchmarks and Controls

| Type | Description
|-|-
| [benchmark](/docs/powerpipe-hcl/benchmark) | Benchmark provides a mechanism for organizing controls into hierarchical structures. 
| [control](/docs/powerpipe-hcl/control) | Controls provide a defined structure and interface for queries that draw a specific conclusion (e.g. 'ok', 'alarm') about each row.


## Dashboards

| Type        | Description                                                                                                                                                           | Valid children types                                                                                               | Allowed at top-level |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ | -------------------- |
| [dashboard](/docs/powerpipe-hcl/dashboard) | Compose resources to meet a reporting requirement. Within the `powerpipe server` dashboard UI, each `dashboard` will be presented as an available item to run.      | `chart`<br/>`container`<br/>`control`<br/>`card`<br/>`flow`<br/>`graph`<br/>`hierarchy`<br/>`image`<br/>`input`<br/>`table`<br/>`text` <br/>`with` | Yes                  |
| [container](/docs/powerpipe-hcl/container) | Lay out reporting resources within a dashboard. Conceptually similar to a dashboard, except it will not be presented as an available item to run within the Powerpipe dashboard UI. | `chart`<br/>`container`<br/>`control`<br/>`card`<br/>`flow`<br/>`graph`<br/>`image`<br/>`input`<br/>`table`<br/>`text` <br/>`with` | Yes                  |
| [card](/docs/powerpipe-hcl/card)      | Display a simple value in different styles e.g. `plain`, `alert` etc. Supports static values, or derived from SQL. | None                                                                                                               | Yes                  |
| [category](/docs/powerpipe-hcl/category)      | Specify display options for `nodes` and `edges` | None | Yes                  |
| [chart](/docs/powerpipe-hcl/chart)     | Visualize SQL data in a chart  e.g. `bar`, `column`, `line`, `pie` etc.                                                                                       | None                                                                                                               | Yes                  |
| [edge](/docs/powerpipe-hcl/edge) | Display an edge to connect nodes on a`flow`, `graph`, or `hierarchy` | None| Yes |
| [flow](/docs/powerpipe-hcl/flow) | Visualize flow data using things such as `sankey`. | `node`, `edge` | Yes |
| [graph](/docs/powerpipe-hcl/graph) | Visualize graph relationships.  | `node`, `edge` | Yes |
| [hierarchy](/docs/powerpipe-hcl/hierarchy) | Visualize hierarchical data using things such as `tree`.| `node`, `edge` | Yes |
| [image](/docs/powerpipe-hcl/image)     | Embed images in reports. Supports static URLs, or can be derived from SQL.                                                                           | None                                                                                                               | Yes                  |
| [input](/docs/powerpipe-hcl/input)     | Enable dynamic dashboards based on user-provided `input`.                                                                                                           | None                                                                                                               | Yes                  |
| [node](/docs/powerpipe-hcl/node) | Display a vertex (node) on a`flow`, `graph`, or `hierarchy` | None| Yes |
| [table](/docs/powerpipe-hcl/table)     | Display tabular data in a dashboard.                                                                                                                                    | None                                                                                                               | Yes                  |
| [text](/docs/powerpipe-hcl/text)      | Add GitHub-flavoured markdown to a dashboard.                                                                                                          | None                                                                                                               | Yes                  |
| [with](/docs/powerpipe-hcl/with)      | Specify additional queries or SQL statements to run **first**, and then pass the query results as arguments to `sql`, `query`, and `node` & `edge` blocks.| None | No |
   