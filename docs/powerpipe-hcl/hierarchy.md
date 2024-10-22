---
title: Hierarchy
sidebar_label: hierarchy
---

# hierarchy

A hierarchy allows visualization of queries using types such as `tree`.    Hierarchies are [node/edge visualizations](/docs/powerpipe-hcl/graph#nodeedge-visualizations).  The data to be displayed is specified using a series of nodes and edges. The nodes define the vertices of the graph, and the edges define the connections between them.

Hierarchy blocks can be declared as named resources at the top level of a mod, or they can be declared as anonymous blocks inside a `dashboard` or `container`, or be re-used inside a `dashboard` or `container` by using a `hierarchy` with `base = <mod>.hierarchy.<hierarchy_resource_name>`.


## Example Usage

<img src="/images/docs/reference_examples/tree_ex_1.png" width="100%" />

```hcl
dashboard "tree_ex_nodeonly" {

  input "vpc" {
    width = 4

    sql = <<-EOQ
      select
        title as label,
        vpc_id as value
      from
        aws_vpc
    EOQ
  }

  hierarchy {
    title = "AWS VPC Subnets by AZ"

    node {
      sql = <<-EOQ
        select
          vpc_id as id,
          vpc_id as title
        from
          aws_vpc
        where
          vpc_id = $1
      EOQ
      args = [self.input.vpc.value]
    }


    node {
      sql = <<-EOQ
        select
          distinct on (availability_zone)
          vpc_id as from_id,
          availability_zone as id,
          availability_zone as title
        from
          aws_vpc_subnet
        where
          vpc_id = $1
      EOQ
      args = [self.input.vpc.value]
      
    }

    node {
      sql = <<-EOQ
        select
          availability_zone as from_id,
          subnet_id as id,
          subnet_id as title
        from
          aws_vpc_subnet
        where
          vpc_id = $1
      EOQ
      args = [self.input.vpc.value]
    }

  }

}
```


## Argument Reference
| Argument | Type | Optional? | Description
|-|-|-|-
| `args` | Map | Optional| A map of arguments to pass to the query. 
| `base` |  Hierarchy Reference		| Optional | A reference to a named `hierarchy` resource that this `hierarchy` should source its definition from. `title` and `width` can be overridden after sourcing via `base`.
| `category` | Block | Optional| [category](#category) blocks that specify display options for nodes with that category.
| `database` | String |  Optional| A database [connection reference](/docs/reference/config-files/connection), [connection string](/docs/powerpipe-hcl/query#connection-strings), or [Pipes workspace](/docs/run/workspaces#implicit-workspaces) to query.  If not specified, the [default database](/docs/run#selecting-a-database ) will be used.
| `edge` | Block | Optional| [edge](/docs/powerpipe-hcl/edge) blocks that define the edges in the hierarchy.
| `node` | Block | Optional| [node](/docs/powerpipe-hcl/node) blocks that define the nodes in the hierarchy.
| `param` | Block | Optional| [param](/docs/powerpipe-hcl/query#param) blocks that defines the parameters that can be passed in to the query.  `param` blocks may only be specified for hierarchies that specify the `sql` argument. 
| `query` | Query Reference | Optional | A reference to a [query](/docs/powerpipe-hcl/query) resource that defines the query to run.  A `hierarchy`  may either specify the `query` argument or the `sql` argument, but not both.
| `--search-path` | String |  Optional| Set a comma-separated list of connections to use as a custom search path for the query
| `--search-path-prefix` | String |  Optional| Set a comma-separated list of connections to use as a prefix to the current search path for the query.
| `sql` |  String	| Optional |  An SQL string to provide data for the `hierarchy`.  A `hierarchy` may either specify the `query` argument or the `sql` argument, but not both.
| `title` |  String	| Optional | A plain text [title](/docs/powerpipe-hcl/dashboard#title) to display for this hierarchy.
| `type` |  String	| Optional | The type of the hierarchy. Can be `tree` or `table`.
| `width` |  Number	| Optional | The [width](/docs/powerpipe-hcl/dashboard#width) as a number of grid units that this item should consume from its parent.
| `with` | Block | Optional| [with](/docs/powerpipe-hcl/with) blocks that define prerequisite queries to run.  `with` blocks may only be specified when the hierarchy is defined as a top-level (mod level), named resource.



## Common Hierarchy Properties

### category

| Property | Type   | Default                                                              | Values                                                                                                                                  | Description |
| -------- | ------ |----------------------------------------------------------------------| --------------------------------------------------------------------------------------------------------------------------------------- | ----------- |
| `color`  | string | The matching color from the default theme for the data series index. | A [valid color value](/docs/powerpipe-hcl/dashboard#color).  This may be a named color, RGB or RGBA string, or a control status color. |  The color to display for this category.           |

## Data Format


## Data Format
Hierarchy data must be provided in a format where each row represents a *node* (vertex), an *edge* (connecting 2 vertices), or both.  Hierarchy queries have the same basic structure as flow queries, but unlike flows, hierarchies are restricted to a strict single-parent structure; a given node may have only a single `from_id`.

Note that both column *names* and their *relative position* are important in hierarchy queries; Powerpipe looks for columns *by name* in the result set, but Postgres `union` queries will *append the rows based on the column's position*, not the name of the column.  ***All the `union` queries must return the same columns, in the same order.***

Significant columns  are:

| Name       | Description
|------------|---------------------------------------------------------------
| `id`       | A unique identifier for the node.  Nodes have an `id`, edges do not.
| `title`    | A title to display for the node.
| `category` | A display category name. Both nodes and edges may specify a `category` to dictate how the item is displayed.  By default, items of the same category are displayed with the same appearance (color), distinct from other categories.  You can specify display options with a [category](#category) block.
| `from_id`  | The `id` of the source side of an edge.
| `to_id`    | The `id` of the destination side of an edge.


Generally speaking, there are 2 data formats commonly used for hierarchies.  It is usually simplest to specify results where each row species a node (with an `id`, and optionally `title`, `category`, and/or `depth`) and an edge, by specifying a `from_id`:  

| from_id | id               | title            | category         |
|---------|------------------|------------------|------------------|
| <null\> | 1                | foo              | root             |
| 1       | 2                | bar              | widget           |
| 1       | 3                | baz              | widget           |
| 2       | 4                | foobar           | fidget           |

Alternatively, you may specify nodes and edges as separate rows.  In this case, nodes will have an `id` and optionally `title`, `category`, and/or `depth`, but `to_id` and `from_id` will be null.  Edges will populate `to_id` and `from_id` and optionally `category`, and will have null `id`, `depth`, and `title`:


| from_id | to_id     | id               | title            | category         |
|---------|-----------|------------------|------------------|------------------|
| <null\> |  <null\>  | 1                | foo              | root             |
| <null\> |  <null\>  | 2                | bar              | widget           |
| <null\> |  <null\>  | 3                | baz              | widget           |
| <null\> |  <null\>  | 4                | foobar           | fidget           |
| 1       |  2        |  <null\>         | <null\>          | widget           |
| 1       |  3        |  <null\>         | <null\>          | widget           |
| 2       |  4        |  <null\>         | <null\>          | fidget           |


## Common Hierarchy Properties

### category

| Property | Type   | Default                                                              | Values                                                                                                                                  | Description |
| -------- | ------ |----------------------------------------------------------------------| --------------------------------------------------------------------------------------------------------------------------------------- | ----------- |
| `color`  | string | The matching color from the default theme for the data series index. | A [valid color value](/docs/powerpipe-hcl/dashboard#color).  This may be a named color, RGB or RGBA string, or a control status color. |  The color to display for this category.           |


## More Examples



### Tree via monolithic query


<img src="/images/docs/reference_examples/tree_ex_1.png" width="100%" />


```hcl
hierarchy {
  type  = "tree"
  title = "AWS VPC Subnets by AZ"
  width = 6

  sql = <<-EOQ

    with vpc as
      (select 'vpc-9d7ae1e7' as vpc_id)

    select
      null as from_id,
      vpc_id as id,
      vpc_id as title,
      0 as depth,
      'aws_vpc' as category
    from
      aws_vpc
    where
      vpc_id in (select vpc_id from vpc)

    union all
    select
      distinct on (availability_zone)
      vpc_id as from_id,
      availability_zone as id,
      availability_zone as title,
      1 as depth,
      'aws_availability_zone' as category
    from
      aws_vpc_subnet
    where
      vpc_id in (select vpc_id from vpc)


    union all
    select
      availability_zone as from_id,
      subnet_id as id,
      subnet_id as title,
      2 as depth,
      'aws_vpc_subnet' as category
    from
      aws_vpc_subnet
    where
      vpc_id in (select vpc_id from vpc)

  EOQ
}
```


### Tree with color by category [monolithic]

<img src="/images/docs/reference_examples/tree_ex_color.png" width="100%" />

```hcl


hierarchy {
  type  = "tree"
  title = "AWS VPC Subnets by AZ"
  width = 6

  category "aws_vpc" {
    color = "blue"
  }

  category "aws_availability_zone" {
    color = "red"
  }

  category "aws_vpc_subnet" {
    color = "green"
  }

  sql = <<-EOQ

    with vpc as
      (select 'vpc-9d7ae1e7' as vpc_id)

    select
      null as from_id,
      vpc_id as id,
      vpc_id as title,
      0 as depth,
      'aws_vpc' as category
    from
      aws_vpc
    where
      vpc_id in (select vpc_id from vpc)

    union all
    select
      distinct on (availability_zone)
      vpc_id as from_id,
      availability_zone as id,
      availability_zone as title,
      1 as depth,
      'aws_availability_zone' as category
    from
      aws_vpc_subnet
    where
      vpc_id in (select vpc_id from vpc)


    union all
    select
      availability_zone as from_id,
      subnet_id as id,
      subnet_id as title,
      2 as depth,
      'aws_vpc_subnet' as category
    from
      aws_vpc_subnet
    where
      vpc_id in (select vpc_id from vpc)

  EOQ
}

```

