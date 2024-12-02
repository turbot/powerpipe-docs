---
title: Writing Controls
---

# Custom Controls

Many controls and benchmarks are available in [mods on the Powerpipe Hub](https://hub.powerpipe.io/). But if these don't meet your needs, Powerpipe makes it easy to create your own [controls](/docs/powerpipe-hcl/control) and [benchmarks](/docs/powerpipe-hcl/benchmark) that are important to *you* and *your organization*, and organize them in a way that reflects your organization's standards and practices.  

## Tutorial

Let's walk through building a simple benchmark that will introduce the key concepts as we go along.

### Prerequisites
For this tutorial, we'll be using [Steampipe](https://steampipe.io) with the [AWS plugin](https://hub.steampipe.io/plugins/turbot/aws):

1. [Download and install Powerpipe](https://powerpipe.io/downloads) for your platform.
2. [Download and install Steampipe](https://steampipe.io/downloads) for your platform.
3. [Install and configure the latest AWS plugin](https://hub.steampipe.io/plugins/turbot/aws).
4. [Start the Steampipe database service](https://steampipe.io/docs/managing/service#starting-the-database-in-service-mode) (`steampipe service start`)


### Create a mod

Powerpipe resources are packaged into [mods](/docs/build).  First, [create a mod](/docs/build/create-mod) for your controls and benchmarks.


### Create a Control
Now let's create a `control`.  Create a new file in the folder called `untagged.pp` and paste in the following code:

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

This snippet defines a control named `s3_untagged`, including an SQL query to find untagged S3 buckets.  Note that the query returns the [required control columns](/docs/powerpipe-hcl/control#required-control-columns) (`resource`, `status`, and `reason`), as well as additional columns, or [dimensions](/docs/powerpipe-hcl/control#additional-control-columns--dimensions), to provide context that is specific to AWS (`region`, `account_id`).

Now let's run our control:
```bash
powerpipe control run s3_untagged
```

<img src="/images/console_out_s3_untagged.png" width="100%" />


Controls provide an easy-to-use mechanism for auditing your environment with Powerpipe.  Benchmarks allow you to group and organize your controls.  Let's add another control to the `untagged.pp`, as well as a benchmark that has both of our controls as children:

```hcl
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
powerpipe benchmark run untagged
```


<img src="/images/console_out_s3_untagged_bench.png" width="100%" />


Benchmarks may also have other benchmarks as children, allowing you to create rich hierarchies of controls.  There are many more examples to explore on the [Powerpipe Hub](https://hub.powerpipe.io/)!