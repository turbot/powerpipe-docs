---
title: Using Powerpipe 
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

Now that Tailpipe is installed, install & configure the [AWS plugin for Tailpipe](https://hub.tailpipe.io/plugins/turbot/aws)

```bash
tailpipe plugin install aws
```

Out of the box, Tailpipe will use the default AWS credentials from your credential file and/or environment variables; if you can run `aws ec2 describe-vpcs`, for example, then you should be able to run the examples. 

>[!NOTE]
> will we refer to the steampipe plugin here, and explain credential_import?

The AWS plugin documentation provides additional examples to [configure your credentials](https://hub.tailpipe.io/plugins/turbot/aws#configuring-aws-credentials), and you can even configure Tailpipe to query [multiple accounts](https://tailpipe.io/docs#:~:text=tailpipe%20to%20query-,multiple%20accounts,-and%20multiple%20regions) and [multiple regions](https://tailpipe.io/docs#:~:text=multiple%20accounts%20and-,multiple%20regions).


## Run a collection against AWS Cloudtrail logs

> [!NOTE]
> Bit of a speed bump here. Should we provide a sample log, show a `.tpc` configured for file source, and explain that doing this for real is just a matter of switching the tpc to an api source (example shown)?

## Run a log analysis benchmark

> [!NOTE]
> Currently tailpipe-mod-aws-detections has no dashboards. Will it, or should we use a different mod to illustrate both dashboards and detections? I am guessing for this doc should we focus on detections and benchmarks? 


Powerpipe [benchmarks](/docs/run/benchmarks) provide a mechanism for defining and running log detections to evaluate threat and error patterns, system performance, and user behavior. Benchmarks are written in simple HCL, and packaged in mods.  It is simple to create your own, but there are also many benchmarks available on the [Powerpipe Hub](https://hub.powerpipe.io/). 

Powerpipe always runs in the context of a [mod](/docs/build/).  A Powerpipe mod is a portable, versioned collection of related Powerpipe resources such as dashboards, benchmarks, and detections defined in HCL, and distributed as simple text files.  Powerpipe loads the mod from the [mod location](/docs/run#mod-location) which defaults to the current directory.

> [!NOTE]
> Will this be the recommended install protocol?

Let's create a new directory for our mod:

```bash
mkdir learn_powerpipe_tailpipe
cd learn_powerpipe_tailpipe
```

Now initialize this mod:
```bash
powerpipe mod init
```

Powerpipe mods may depend on other mods.  When running Powerpipe from a mod, all dashboards in the mod and its direct dependencies will be available to run. Let's install the AWS Detections.

```bash
powerpipe mod install github.com/turbot/tailpipe-mod-aws-detections
```

Let's run the XXX benchmark:
```bash
powerpipe benchmark run aws_detections.benchmark.mitre_v151_ta0001
```

>[!NOTE]
> redo for tp

## Create your own dashboards and benchmarks

The [Powerpipe Hub](https://hub.powerpipe.io/) contains ready-made [[dashboards and]]? benchmarks that you can simply install and run. But Powerpipe also makes it simple to [write your detections and benchmarks](/docs/build/writing-detections), and [build dashboards](/docs/build/writing-dashboards) to analyze your data and share with others! 

We've already [created our own mod](/docs/build/create-mod) and installed a dependency mod, AWS Detections.  Now let's add our own dashboard and benchmark.  Create a file named `learn.pp` in your mod's directory, and paste in the following HCL code:

>[!NOTE]
> redo for tp

