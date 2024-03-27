---
title: Container
sidebar_label: container
---

# container

A container groups and arranges related items within a dashboard. For example, you may want to group together a number of different charts for a specific AWS service.

Containers can only be declared as anonymous blocks inside a `dashboard` or `container`.


## Example Usage

<img src="/images/docs/reference_examples/container_ex_1.png" width="100%" />

```hcl

container {
  title = "Side by Side Container Example"
  container {
    width = 6

    card {
      sql  = "select 1 as \"Left Side\""
    }
     card {
      sql  = "select 2 as \"Left Side\""
    }
  }

  container {
    width = 6
    card {
      sql  = "select 1 as \"Right Side\""
    }
     card {
      sql  = "select 2 as \"Right Side\""
    }
  }
}
```


## Argument Reference
| Argument | Type | Optional? | Description
|-|-|-|-
| `chart`        | Block	| Optional | [chart](/docs/powerpipe-hcl/chart)  blocks to visualize SQL data in a number of ways e.g. `bar`, `column`, `line`, `pie` 
| `container` |  Block	| Optional |  [container](/docs/powerpipe-hcl/container) blocks to lay out related components together in a dashboard. 
| `flow` | Block	| Optional |  [flow](/docs/powerpipe-hcl/flow)  blocks to visualize flows using types such as `sankey`. 
| `hierarchy` | Block	| Optional |  [hierarchy](/docs/powerpipe-hcl/hierarchy)  blocks to visualize hierarchical data using types such as `tree`. 
| `image`     | Block	| Optional | [image](/docs/powerpipe-hcl/image)    blocks to embed images in dashboards. Supports static URLs, or can be derived from SQL.                                                                               
| `input`     | Block	| Optional | [input](/docs/powerpipe-hcl/input) blocks to make dynamic dashboards based on user-provided input.     
| `table`      | Block	| Optional | [table](/docs/powerpipe-hcl/table)   blocks to show tabular data in a dashboard.
| `text`       | Block	| Optional | [text](/docs/powerpipe-hcl/text) blocks to add GitHub-flavoured markdown to a dashboard.      
| `title` |  String	| Optional | A plain text [title](/docs/powerpipe-hcl/dashboard#title) to display for this container.
| `width` |  Number	| Optional | The [width](/docs/powerpipe-hcl/dashboard#width) as a number of grid units that this item should consume from its parent.
