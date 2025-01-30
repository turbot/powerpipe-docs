---
title: query
sidebar_label: query
---

# query
Queries are defined in [mods](/docs/build). They define common SQL statements that may be used alone, or referenced in other blocks like controls and charts. Powerpipe mods may use Steampipe's Postgres engine or Tailpipe's DuckDB engine.

Note that a Powerpipe `query` is NOT a database resource. It does not create a view, stored procedure, etc. `query` blocks are interpreted by and executed by Powerpipe, and are only available by way of these engines, not with 3rd party tools.

## Example Usage

### In a Powerpipe mod using Steampipe

```hcl
query "plus_size_instances" {
  title = "EC2 Instances xlarge and bigger"
  sql   = "select * from aws_ec2_instance where instance_type like '%xlarge'"
}
```

### In a Powerpipe mod using Tailpipe

```hcl
query "audit_logs_detect_failed_workflow_actions" {
  sql = <<-EOQ
    select
      *
    from
      github_audit_log
    where
      action = 'workflows.completed_workflow_run'
    order by
      tp_timestamp desc;
  EOQ
}
```


You can run a query by its name with the `powerpipe query run` command:
```bash 
$ powerpipe query run plus_size_instances
```

## Argument Reference

| Argument |Type | Required? | Description
|-|-|-|-
| `database` | String |  Optional| A database [connection reference](/docs/reference/config-files/connection), [connection string](/docs/powerpipe-hcl/query#connection-strings), or [Pipes workspace](/docs/run/workspaces#implicit-workspaces) to query.  If not specified, the [default database](/docs/run#selecting-a-database ) will be used.
| `description` | String |  Optional| A description of the query.
| `documentation` | String (Markdown)| Optional | A markdown string containing a long form description, used as documentation for the mod on hub.powerpipe.io. 
| `param` | Block | Optional| A [param](#param) block that defines the parameters that can be passed in to the query.  
| `--search-path` | String |  Optional| Set a comma-separated list of connections to use as a custom search path for the query
| `--search-path-prefix` | String |  Optional| Set a comma-separated list of connections to use as a prefix to the current search path for the query.
| `sql` | String | Required | SQL statement to define the query.
| `tags` | Map | Optional | A map of key:value metadata for the query, used to categorize, search, and filter.  The structure is up to the mod author and varies by mod. 
| `title` | String | Optional | A display title for the query.



### Database
Mods are typically written for a specific database engine and schema.  Powerpipe allows you to set a [default database connection](/docs/build/mod-database) for your mod.  Any `query`, `control`, `chart`, or other query-provider resource that does not specifically set the `database` argument will use this database.  Because the majority of mods are currently written for [Steampipe](https://steampipe.io) databases, the default database is set to the local Steampipe instance if it is not specified for the mod (`postgres://steampipe@localhost:9193/steampipe`). It is generally advisable *not* to set the `database` on the mod resources in the code, and instead use the mod default.  

As with the [default database connection](/docs/build/mod-database),  the `database` may be:
  - A database [connection reference](/docs/reference/config-files/connection), eg `connection.steampipe.default`.
  - A [connection string](/docs/powerpipe-hcl/query#connection-strings), eg `sqlite://myfile.db`
  - A [Pipes workspace](/docs/run/workspaces#implicit-workspaces), eg `acme/anvils`


#### Connection Strings

The connection string syntax for `database` argument is the same whether you set it in the `mod` or the `query`, `card`, `chart`, etc.  

[[ do we want to update these to show generic connections like these, and steampipe/tailpipe connections like steampipee.connection.default / tailpipe connection default? is it not going to possible with powerpipe to use literal connection strings, and if so should there be 4 sections: steampipe-backed mod, any postgres, tailpipe-backed mod, any duckdb ]]

##### Steampipe, or any Postgres db.
Powerpipe can connect to any Postgres database, including the one bundled with Steampipe.  The Postgres `database` follows the standard URI syntax supported by `psql` and `pgcli`:
```bash
postgresql://[user[:password]@][host][:port][/dbname][?param1=value1&...]
```

For example:
```hcl
query "my_query" {
  database = "postgresql://myusername:mypassword@acme-prod.apse1.db.cloud.turbot.io:9193/aaa000"
  sql               = "select * from my_table;" 
}
```

##### Tailpipe, or any DuckDB db.

The DuckDB connection string is the path to a DuckDB database file:
```bash
duckdb:path/to/file
```

The path is relative to the [mod location](/docs/run#mod-location), and `//` is optional after the scheme, thus the following are equivalent:
```hcl
database = "duckdb:./my_ducks.db"
database = "duckdb://./my_ducks.db"
database = "duckdb://my_ducks.db"
```


For example:
```hcl
query "my_query" {
  database = "duckdb:./my_ducks.db
  sql      = "select * from my_table;" 
}
```

##### MySQL
The MySQL connection string supports the syntax of the [GO SQL driver for MySQL](https://github.com/go-sql-driver/mysql?tab=readme-ov-file#examples):

```bash
mysql://[user[:password]@]network-location[:port][/dbname][?param1=value1&...]
```

```hcl
query "my_query" {
  database = "mysql://root:my_pass@tcp(localhost)/mysql"
  sql      = "select * from my_table;" 
}
```


##### SQLite
The SQLite `database` connection string is the path to a SQLite database file:

```bash
sqlite:path/to/file
```

The path is relative to the [mod location](/docs/run#mod-location), and `//` is optional after the scheme, thus the following are equivalent:
```hcl
database = "sqlite:./my_sqlite_db.db"
database = "sqlite://./my_sqlite_db.db"
database = "sqlite://my_sqlite_db.db"
```

For example:
```hcl
query "my_query" {
  database = "sqlite:./my_sqlite_db.db"
  sql      = "select * from my_table;" 
}
```


### param
One or more `param` blocks may optionally be used in a query to define parameters that the query accepts.  Note that the SQL statement only supports positional arguments (`$1`, `$2`, ...) and that the param blocks are assigned in order -- the first param block describes `$1`, the second describes `$2`, etc.

| Name | Type| Description
|-|-|-
| `description` | String | A description of the parameter.
| `default`     | Any | A value to use if no argument is passed for this parameter when the query is run.


```hcl
variable "max_access_key_age" {
  type = number
}

query "old_access_keys" {
  sql = <<-EOT
    select
      access_key_id,
      user_name,
      create_date,
      age(create_date) as age
    from
      aws_iam_access_key
    where
      create_date < NOW() - ($1 ::numeric || ' days')::interval;
  EOT

  param "max_days" {
    default     = var.max_access_key_age
    description = "The maximum allowed key age, in days."
  } 
}
```


---

# Query-based Resources

There are many Powerpipe mod elements that execute a query, including `control` and most dashboard visualization elements (`card`, `chart`, `node`, `edge`, `graphs` etc). These resources essentially implement the same interface:
  - They have an `sql` argument for specifying an SQL string to execute
  - They have a `query` argument for referencing a `query` to execute
  - They require the user to set either `sql` or `query`, but both may not be specified.
  - They have an optional `param` argument to specify parameters that the `sql` accepts
  - They have an optional `args` argument to specify what arguments to pass to the `query` or `sql`


## Query v/s SQL

When using a query-based resource, you **must** specify either the `sql` or `query` argument, but not both.

The difference between these arguments is somewhat subtle.

The `sql` argument is a simple *string* that defines a SQL statement to execute. This allows you to inline the statement in the resource definition:

```hcl
card {
  sql = <<-EOQ
    select
      count(*) as "Buckets"
    from
      aws_s3_bucket
  EOQ

  width = 2
}
```

The `query` argument is a *reference to a named `query` resource* to run. This allows you to reuse a query from multiple other resources:

```hcl
card {
  width = 2
  query = query.bucket_count
}

query "bucket_count" {
  sql = <<-EOQ
    select
      count(*) as "Buckets"
    from
      aws_s3_bucket
  EOQ

}
```


## Params v/s Args

Query-based resources allow you to specify parameters that the `sql` accepts and to specify what arguments to pass to the `query` or `sql`.  The difference between `param` and `args` is a common source of confusion.

A `param` block is used to define a parameter *that the `sql` accepts* - It is a definition of what you can pass. The `args` block is used to specify what *values to pass to the `query` or `sql`* - these are the actual values to use when running the query.

`param` blocks are only used when the `sql` argument is specified and the SQL statement includes parameters (`$1`, `$2`):

```hcl
card "bucket_count_for_region" {
  sql = <<-EOQ
    select
      count(*) as "Buckets"
    from
      aws_s3_bucket
    where
      region = $1
  EOQ

  param "region" {
    default = "us-east-1"
  }

  width = 2
}
```

***You can only specify `param` blocks for resources that are defined as top-level named resources in your mod.***  It would not make sense to specify a `param` block for an anonymous resource that is defined in a dashboard since you cannot reference it anyway.

The `args` argument is used to pass values to a query at run time.  If the `query` resource has parameters defined, then the `args` argument is used to pass values to the query:

```hcl
dashboard "s3_dashboard" {
  card {
    width = 2
    query = query.bucket_count_for_region

    args = {
      region = "us-east-1"
    }
  }
}

query "bucket_count_for_region" {
  sql = <<-EOQ
    select
      count(*) as "Buckets"
    from
      aws_s3_bucket
    where
      region = $1
  EOQ

  param "region" {}
}
```

Note that most query-based resources can be defined as top-level resources and reused with `base`, and you can pass arguments to them in the same manner:

[[ I have added a tailpipe example. should we have these in 2 sections? and what will our terminology be? (steampipe|tailpipe)-backed mods? something else?]]

```hcl
dashboard "s3_buckets" {

  card {
    base = card.bucket_count_for_region
    args = {
      region = "us-east-1"
    }
  }

  card {
    base = card.bucket_count_for_region
    args = {
      region = "us-east-2"
    }
  }
}

card "bucket_count_for_region" {
  sql = <<-EOQ
    select
      count(*) as "Buckets"
    from
      aws_s3_bucket
    where
      region = $1
  EOQ

  param "region" {
    default = "us-east-1"
  }

  width = 2
}

card "failed_workflow_actions

  sql = <<-EOQ
    select
      count(*)
    from
      github_audit_log
    where
      action = 'workflows.completed_workflow_run'
```

While `param` blocks are ***recommended*** when SQL statements define parameters, they are not required -- you won't be able to pass arguments by name, but you can still pass them positionally using the array format of the `args` argument:

```hcl
dashboard "s3_dashboard" {
  card {
    width = 2
    query = query.bucket_count_for_region

    args  = ["us-east-1"]
  }
}

query "bucket_count_for_region" {
  sql = <<-EOQ
    select
      count(*) as "Buckets"
    from
      aws_s3_bucket
    where
      region = $1
  EOQ
}
```