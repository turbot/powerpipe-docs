---
title: Using Powerpipe with Tailpipe
sidebar_label: With Tailpipe
---

#  Using Powerpipe with Tailpipe

## Prerequisites

To get started, you will need to install Powerpipe, Tailpipe, and the AWS plugin for Tailpipe.

Powerpipe [does not require Tailpipe](/docs/run#selecting-a-database); it can work with any PostgreSQL, MySQL, SQLite, or DuckDB database. [Mods](/docs/build) are written for a specific SQL syntax and database schema, however, and the examples in this article require a Tailpipe database with the AWS plugin.  


First, install [Powerpipe](https://powerpipe.io/downloads).  

```bash+macos
brew install turbot/tap/powerpipe
```

```bash+linux 
sudo /bin/sh -c "$(curl -fsSL https://powerpipe.io/install/powerpipe.sh)"
```

Next, install [Tailpipe](https://tailpipe.io/downloads). 

```bash+macos
brew install turbot/tap/tailpipe
```

```bash+linux
sudo /bin/sh -c "$(curl -fsSL https://tailpipe.io/install/tailpipe.sh)"
```

Now that Tailpipe is installed, install the [AWS plugin for Tailpipe](https://hub.tailpipe.io/plugins/turbot/aws)

```bash
tailpipe plugin install aws
```

Out of the box, Tailpipe will use the default AWS credentials from your credential file and/or environment variables; if you can run `aws ec2 describe-vpcs`, for example, then you should be able to run the examples. 

The AWS plugin documentation provides additional examples to [configure your credentials](https://hub.tailpipe.io/plugins/turbot/aws#configuring-aws-credentials), and you can even configure Tailpipe to query [multiple accounts](https://tailpipe.io/docs#:~:text=tailpipe%20to%20query-,multiple%20accounts,-and%20multiple%20regions) and [multiple regions](https://tailpipe.io/docs#:~:text=multiple%20accounts%20and-,multiple%20regions).


## Run a collection against AWS Cloudtrail logs

Here's a configuration that uses the `aws_s3_bucket` source, and assumes you have the correct AWS credentials to access the bucket.

```
 connection "aws" "dev" {
  profile = "SSO-Admin-605...13981"
  regions = ["*"]
}

partition "aws_cloudtrail_log" "dev" {
  source "aws_s3_bucket" {
    connection = connection.aws.dev
    bucket     = "aws-cloudtrail-logs-6054...81-fe67"
    prefix     = "AWSLogs/6054...81/CloudTrail/us-east-1/2024/12"
    region     = "us-east-1"
    extensions = [".gz"]
  }
}
```

Run the command:

```
tailpipe collect aws_cloudtrail_log.dev
```

Observe the output:

```
Collection complete.

Artifacts discovered: 1031. Artifacts downloaded: 1031. Artifacts extracted: 1031. Rows enriched: 5456. Rows converted: 5456. Errors: 0.

Compacted 2 files into 1 files. (232 files did not need compaction.)
```

**Artifacts discovered**: the number of `.gz` files added to the bucket since the last collection.

**Rows enriched**: the number of log lines in the unzipped `.gz` files.

**Compacted 2 files**: when collection results in more than 1 parquet file for a given day, Tailpipe compacts to a single file for that day.

### Multiple sources and partitions

If you had also downloaded some logs, you could collect them into another partition using the `file_system` source. The collection command would be `tailpipe collect aws_cloudtrail_log.dev-file`.

```
 connection "aws" "dev" {
   ...
}

partition "aws_cloudtrail_log" "dev" {
  ...
}

partition "aws_cloudtrail_log" "dev-file" {
     source "file_system"  {  # i think will be this not file_system?
        paths = ["/path/to/logs"]
        extensions = [".gz"]
    }
}
```

 You can define many of these partition/source combinations.


## Run a benchmark in the CLI


Powerpipe [benchmarks](/docs/run/benchmarks) provide a mechanism for defining and running log detections to evaluate threat and error patterns, system performance, and user behavior. Benchmarks are written in simple HCL, and packaged in mods.  It is simple to create your own, but there are also many benchmarks available on the [Powerpipe Hub](https://hub.powerpipe.io/). 

Powerpipe always runs in the context of a [mod](/docs/build/).  A Powerpipe mod is a portable, versioned collection of related Powerpipe resources such as dashboards, benchmarks, and detections defined in HCL, and distributed as simple text files.  Powerpipe loads the mod from the [mod location](/docs/run#mod-location) which defaults to the current directory.

Let's create a new directory for our mod:

```bash
mkdir learn_powerpipe_tailpipe
cd learn_powerpipe_tailpipe
```

Now install the [Tailpipe AWS Detections](https://hub.tailpipe.io/mods/turbot/tailpipe-mod-aws_detections) mod.

```bash
powerpipe mod install github.com/turbot/tailpipe-mod-aws-detections
```

Let's run the `cloudtrail_logs_cloudtrail_detection` benchmark.
```bash
powerpipe benchmark run aws_detections.cloudtrail_logs_cloudtrail_detection
```

>[!NOTE]
> output now json, will be ???

## Run a benchmark in a browser.

Start the server:
```
powerpipe server
```

Visit `localhost:9033` in a browser. 

![](/images/docs/learn/tailpipe-benchmark-detect-kms-key-updated.png)

The Tailpipe mod has detected 14 potential issues, of which 5 are detectionsd related to updates to KMS keys. If you know that *ExampleUser* is non-malicious you can exclude those 5 rows with a single click on any row in the `actor` column. 

![](/images/docs/learn/tailpipe-benchmark-detect-kms-key-updated-2.png)

Then you can continue to explore the remaining potential issues in other detection categories.

## Create your own dashboards and benchmarks

The [Powerpipe Hub](https://hub.powerpipe.io/) contains ready-made [[dashboards and]]? benchmarks that you can simply install and run. But Powerpipe also makes it simple to [write your detections and benchmarks](/docs/build/writing-detections), and [build dashboards](/docs/build/writing-dashboards) to analyze your data and share with others! 

We've already [created our own mod](/docs/build/create-mod) and installed a dependency mod, AWS Detections.  Now let's add our own dashboard and benchmark.  Create a file named `learn.pp` in your mod's directory, and paste in the following HCL code:

>[!NOTE]
> redo for tp

