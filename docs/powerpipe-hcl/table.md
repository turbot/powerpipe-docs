---
title: Table
sidebar_label: table
---

# table

A table displays query results in a dashboard.

Table blocks can be declared as named resources at the top level of a mod, or they can be declared as anonymous blocks inside a `dashboard` or `container`, or be re-used inside a `dashboard` or `container` by using a `table` with `base = <mod>.table.<table_resource_name>`.


## Example Usage
<img src="/images/docs/reference_examples/table_ex_1.png"  />

```hcl
table {
  title = "S3 Buckets"
  width = 4

  sql   = <<-EOQ
    select
        title,
        arn,
        region
    from
        aws_s3_bucket
    order by
        title
  EOQ
}
```



## Argument Reference
| Argument | Type              | Optional? | Description                                                                                                                                                   |
|----------|-------------------|-----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `args` | Map | Optional| A map of arguments to pass to the query.
| `base`   | Table Reference 	 | Optional  | A reference to a named `table` resource that this `table` should source its definition from. `title` and `width` can be overridden after sourcing via `base`. |
| `column` | String	           | Optional  | A named block matching the name of the column you wish to configure. See [column](#column).                                                                   |
| `database` | String |  Optional| A database [connection reference](/docs/reference/config-files/connection), [connection string](/docs/powerpipe-hcl/query#connection-strings), or [Pipes workspace](/docs/run/workspaces#implicit-workspaces) to query.  If not specified, the [default database](/docs/run#selecting-a-database ) will be used.
| `param` | Block | Optional| A [param](/docs/powerpipe-hcl/query#param) block that defines the parameters that can be passed in to the query.  `param` blocks may only be specified for tables that specify the `sql` argument. 
| `query` | Query Reference | Optional | A reference to a [query](/docs/powerpipe-hcl/query) resource that defines the query to run.  A `table`  may either specify the `query` argument or the `sql` argument, but not both.
| `--search-path` | String |  Optional| Set a comma-separated list of connections to use as a custom search path for the query
| `--search-path-prefix` | String |  Optional| Set a comma-separated list of connections to use as a prefix to the current search path for the query.
| `sql` |  String	| Optional |  An SQL string to provide data for the `table`.  A `table` may either specify the `query` argument or the `sql` argument, but not both.
| `title`  | String	           | Optional  | A plain text [title](/docs/powerpipe-hcl/dashboard#title) to display for this table.                                                                |
| `type`   | String	           | Optional  | The type of the table. Can be `table` (default) or `line`. `line` view transposes each row into a key/value pair (column name/column value) item view         |
| `width`  | Number 	          | Optional  | The [width](/docs/powerpipe-hcl/dashboard#width) as a number of grid units that this item should consume from its parent.                           |


## Table Properties

### column

| Property  | Type   | Default | Values                                                                                                        |
|-----------| ------ |---------|---------------------------------------------------------------------------------------------------------------|
| `display` | string | `all`   | `all` (default) will show the column at all breakpoints. `none` will never show the column.                   |
| `wrap`    | string | `none`  | `all` will wrap the column contents at all breakpoints. `none` (default) will never wrap the column contents. |
| `href`    | string |         | A url that the column should link to.  The `href` may use a [jq template](#jq-templates) to dynamically generate the link for each row.  |

#### jq Templates
The `href` argument allows you to specify a [jq](https://stedolan.github.io/jq/) template to dynamically generate a hyperlink from the data in the row. To use a jq template, enclose the jq in double curly braces (`{{ }}`).  

Powerpipe will pass each row of data to jq in the same format that is returned by [powerpipe query json mode output](/docs/reference/cli/query#powerpipe-query-run), where the keys are the column names and the values are the data for that row. 

For example, this query:
```sql
select 
  instance_id, 
  region, 
  sg->>'GroupId' as security_group
from 
  aws_ec2_instance, 
  jsonb_array_elements(security_groups) as sg

```

will present rows to the jq template in this format:
```json
{
  "instance_id": "i-03d11d111b1407bbc",
  "region": "us-east-2",
  "security_group": "sg-01ee40ea54e0fa089"
 }
```

which you can then use in a jq template in the `href` argument:

```hcl
table {
  title = "Attached Security Groups"
  width = 4
  sql = <<-EOQ
    select 
      instance_id, 
      region, 
      sg->>'GroupId' as security_group
    from 
      aws_ec2_instance, 
      jsonb_array_elements(security_groups) as sg      
  EOQ

  column "security_group" {
    href = "${dashboard.aws_vpc_security_group_detail.url_path}?input.security_group_id={{.'security_group' | @uri}}"
  }
}
```

Refer to [JQ [Escaping & Interpolation](/docs/powerpipe-hcl/dashboard#jq-escaping--interpolation) for more advanced examples.
## More Examples


### Table with options for columns
<img src="/images/docs/reference_examples/table_ex_columns.png" width="400pt"/>

```hcl
table {
  title = "S3 Buckets"

  width = 3
  sql = <<-EOQ
    select
        title,
        arn,
        region
    from
        aws_s3_bucket
    where name like '%vandelay%'
    order by
        title
  EOQ 

  column "region" {
    display = "none"
  }
  
  column "arn" {
    wrap = "all"
  }
}
```


### Table in line mode
<img src="/images/docs/reference_examples/table_ex_line.png" width="300pt"/>

```hcl
table {
  type  = "line"
  title = "S3 Buckets"
  width = 4

  sql   = <<-EOQ
    select
        title,
        arn,
        region
    from
        aws_s3_bucket
    where 
      name = 'vandelay-industries-elaines-bucket'
    order by
        title
  EOQ
}
```



### Table with links
<img src="/images/docs/reference_examples/table_with_links_ex.png" />

```hcl
table {
  title = "Attached Security Groups"
  width = 4
  sql = <<-EOQ
    select 
      instance_id, 
      region, 
      sg->>'GroupId' as security_group
    from 
      aws_ec2_instance, 
      jsonb_array_elements(security_groups) as sg      
  EOQ

  column "security_group" {
    href = "${dashboard.aws_vpc_security_group_detail.url_path}?input.security_group_id={{.'security_group' | @uri}}"
  }
}
```
