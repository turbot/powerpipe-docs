---
title: Passing Parameters
---

# Passing Parameters to Queries & Controls

A query may optionally define **parameters**.  When executing the query, you can specify values for the parameters to be used for the query execution.


## Using Parameters in Queries
Variable usage and interpolation in Powerpipe is based on and conforms to Terraform.  Special consideration must be made for passing variables into queries, however, as both the HCL parser AND the SQL parser must account for the variables.

When defining a query, you may use positional parameters (`$1`, `$2`, `$3`, ...) in the query definition.  For each of these positional parameters, you should define a `param` block that names and describes the parameter. Note that Powerpipe will assign the parameters in the order that the `param` blocks are defined - the first `param` block describes `$1`, the second describes `$2`, etc:

```hcl
query "instances_in_state" {
  sql = "select instance_id, instance_state from aws_ec2_instance where instance_state = $1;" 

  param "state" {
    default = "stopped"
  } 
}
```

You can also pass list values as parameters, and they will converted to PostgreSQL arrays in the query:

```hcl
query "instances_in_states" {
  sql = "select instance_id, instance_state from aws_ec2_instance where instance_state = any($1);" 

  param "states" {
    default = ["stopped", "running"]
  } 
}
```

## Passing Arguments

You can run a query or control by name from the command line.  If the query provides defaults for all the parameters, you can run it without arguments in the same way you would run a query or control that takes no parameters, and it will run with the default values:

```bash
powerpipe query run instances_in_state
```

If the query does not provide a default, or you wish to run the query with a different value, you can pass an argument to the query with one or more `--arg` arguments.

You can pass them by name:
```bash
powerpipe query run instances_in_state --arg state='running'
```

Or by position. If no argument name is provided, the arguments will be passed to the query in the order they are passed to the command (the first `--arg` as `$1`, the second as `$2`, etc.):
```bash
powerpipe query run instances_in_state --arg 'running'
```

Likewise, when specifying arguments in HCL, you can pass them by name (as a map):
```hcl
control "running_instances" {
  title       = "EC2 instances that are running"
  query       = query.instances_in_state
  args        = {
    "state"   = "running"
  }
}
```

Or by position (as a list):
```hcl
control "running_instances_list" {
  title       = "EC2 instances that are running"
  query       = query.instances_in_state
  args        = ["running"]
}
```

## Using Parameters in Controls, Charts, and other resources

Controls, charts, cards, and many other resources allow you to refer to a parameterized query with the `query` argument, and you can pass arguments to the `query` in the `args` argument:

```hcl
query "instances_invalid_state" {
  sql = <<-EOT
    select 
      arn as resource,
      case
        when instance_state = any($1) then 'alarm'
        else 'ok'
      end as status,
      instance_id || ' is ' || instance_state as reason,
      region,
      account_id
    from
      aws_ec2_instance
  EOT
  param "invalid_states" {
    default = ["running"]
  } 
}


control "stopped_instances" {
    title       = "EC2 instances that are stopped"
    query       = query.instances_invalid_state
    args        = {
      "invalid_states"   = ["stopped", "stopping"]
    }
}
```

Alternatively, you may specify inline `sql`, and define `param` blocks as you would for a query:

```hcl
control "stopped_instances_inline" {
  title = "Stopped EC2 instances"
  sql   = <<-EOT
    select 
      arn as resource,
      case
        when instance_state = any($1) then 'alarm'
        else 'ok'
      end as status,
      instance_id || ' is ' || instance_state as reason,
      region,
      account_id
    from
      aws_ec2_instance
  EOT
  param "invalid_states" {
    default = ["stopped", "stopping"]
  } 
}
```

Note that you may *either* reference a query object with the `query` argument *or* use inline SQL with the `sql` argument from your control, *but not both*, and the behavior is subtly different, as can be seen in the examples above:
- The `query` argument is a reference to a `query` resource. You cannot define parameters (`param` blocks) for the control, but you can pass them as arguments (`args`) *to* the query if *the query* has parameters defined.
- The `sql` argument is a string.  When the control specifies a SQL string, it essentially behaves like a query, and thus you can define the parameters that it accepts (in `param` blocks) in the same manner as a `query` resource.  


## Using Parameters with Variables

It is common for arguments and parameter defaults to refer to [input variables](/docs/build/mod-variables), so that users of the mod can change the values without modifying the code:

```hcl
variable "bad_states" {
  type    = list(string)
  default = ["stopped", "stopping"]
}


control "instances_in_bad_states" {
    title       = "EC2 instances that are stopped"
    query       = query.instances_invalid_state
    args        = {
      "invalid_states"   = var.bad_states
    }
}
```



## Using Parameters with Inputs

It is common for arguments to refer to dashboard [input](/docs/powerpipe-hcl/input) elements, allowing you to create rich, dynamic, interactive reports:

```hcl
dashboard "inputs_param_example_dashboard" {
  title = "Inputs/Params Example Dashboard"

  input "region" {
  sql = <<-EOQ
    select
      distinct region as label,
      region as value
    from
      aws_region
    order by
      region;
  EOQ    
  width = 3
  }
  
  table {
    sql = <<-EOQ
      select
        name,
        versioning_enabled
      from
        aws_s3_bucket
      where
        region = $1
    EOQ
    args =  [self.input.region.value]
  }
}

```