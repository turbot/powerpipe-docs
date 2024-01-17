---
title: Writing Controls
sidebar_label: Writing Controls
---

# Custom Controls

Powerpipe makes it easy to create your own [controls](/docs/powerpipe-hcl/control) and [benchmarks](/docs/powerpipe-hcl/benchmark).  This allows you to define the controls that are important to *you* and *your organization*, and organize them in a way that reflects your organization's standards and practices.  (Of course there are controls and benchmarks already available in [mods on the Powerpipe Hub](https://hub.powerpipe.io/mods) as well if you don't want to write your own).

## Tutorial
For this tutorial we'll be using the Powerpipe [AWS plugin](https://hub.powerpipe.io/plugins/turbot/aws).  If you have not already, download and install the latest AWS plugin:
```bash
powerpipe plugin install aws
```

### Create a mod

First, lets create a new directory for our mod: 

```bash
mkdir untagged
cd untagged
```

Powerpipe will look for a mod definition in the current directory by default.  Lets create a mod in our new folder:
```bash
powerpipe mod init
```

The `powerpipe mod init` command creates a `mod.sp` file in the current directory, and names the mod `local`.  Edit the `mod` name and `title`:
```hcl
mod "untagged_example" {
  title = "Untagged Examples"
}
```

Now lets create a `control`.  Create a new file in the folder called `untagged.sp` and paste in the following code:

```hcl
control "s3_untagged" {
  title = "S3 Untagged"
  sql = <<EOT
    select
      arn as resource,
      case
        when tags is not null then 'ok'
        else 'alarm'
      end as status,
      case
        when tags is not null then name || ' has tags.'
        else name || ' has no tags.'
      end as reason,
      region,
      account_id
    from
      aws_s3_bucket
    EOT
}
```

This snippet defines a control named `s3_untagged`, including a sql query to find untagged S3 buckets.  Note that the query returns the [required control columns](/docs/powerpipe-hcl/control#required-control-columns) (`resource`, `status`, and `reason`), as well as additional columns, or [dimensions](/docs/powerpipe-hcl/control#additional-control-columns--dimensions), to provide context that is specific to AWS (`region`, `account_id`).

Now lets run our control:
```bash
powerpipe check control.s3_untagged
```

<img src="/images/console_out_s3_untagged.png" width="100%" />


Controls provide an easy to use mechanism for auditing your environment with Powerpipe.  Benchmarks allow you to group and organize your controls.  Lets add another control to the `untagged.sp`, as well as a benchmark that has both of our controls as children:

```hcl
control "s3_untagged" {
  title = "S3 Untagged"
  sql = <<EOT
    select
      arn as resource,
      case
        when tags is not null then 'ok'
        else 'alarm'
      end as status,
      case
        when tags is not null then name || ' has tags.'
        else name || ' has no tags.'
      end as reason,
      region,
      account_id
    from
      aws_s3_bucket
    EOT
}

control "lambda_untagged" {
  title = "Lambda Untagged"
  sql = <<EOT
    select
      arn as resource,
      case
        when tags is not null then 'ok'
        else 'alarm'
      end as status,
      case
        when tags is not null then name || ' has tags.'
        else name || ' has no tags.'
      end as reason,
      region,
      account_id
    from
      aws_lambda_function
    order by reason
    EOT
}

benchmark "untagged" {
  title = "Untagged"
  children = [
    control.lambda_untagged,
    control.s3_untagged,
  ]
}

```


Now we can run both of our controls via the benchmark:
```bash
powerpipe check benchmark.untagged
```


<img src="/images/console_out_s3_untagged_bench.png" width="100%" />


Benchmarks may have also have other benchmarks as children, allowing you to create rich hierarchies of controls. 