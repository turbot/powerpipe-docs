---
title: Using Powerpipe with Tailpipe
sidebar_label: With Tailpipe
---

#  Using Powerpipe with Tailpipe

Powerpipe is the engine for visualizing Tailpipe [detections](/docs/powerpipe-hcl/detection). Let's see how that works.

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


## Collect log data

Powerpipe mods for Tailpipe work with tables built from log data collected by Tailpipe. The Tailpipe docs show you how to [configure](https://tailpipe.io/docs#configure-data-collection) to configure the AWS plugin for Tailpipe and then [collect](https://tailpipe-io.vercel.app/docs#configure-data-collection) log data. Follow those steps create the table `aws_cloudtrail_log`, and verify that you can run the sample queries shown there.


## Run a benchmark

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

Start the server:
```
powerpipe server
```

Visit `localhost:9033` in a browser. 

![](/images/docs/learn/tailpipe-benchmark-detect-kms-key-updated.png)

The Tailpipe mod has detected 14 potential issues, of which 5 are detectionsd related to updates to KMS keys. If you know that *ExampleUser* is non-malicious you can exclude those 5 rows with a single click on any row in the `actor` column that matches *ExampleUser*.

![](/images/docs/learn/tailpipe-benchmark-detect-kms-key-updated-2.png)

Then you can continue to explore the remaining potential issues in other detection categories.

## Create your own benchmarks, detections, and dashboards

The [Powerpipe Hub](https://hub.powerpipe.io/) contains ready-made benchmarks with sets of detections that you can simply install and run. But Powerpipe also makes it simple to [write your detections and benchmarks](/docs/build/writing-detections), and [build dashboards](/docs/build/writing-dashboards) to analyze your log data and share with others! 
