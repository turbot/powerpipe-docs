---
id: learn
title: Learn Powerpipe
slug: /
---

# Learn Powerpipe: Dashboards for DevOps!

Powerpipe is an open-source tool from Turbot that enables DevOps teams to:

- [Visualize cloud infrastructure](#visualize-cloud-infrastructure). Use pre-built dashboards — for AWS, Azure, GCP, Kubernetes, and more — to visualize your cloud resources and answer common questions about resource quantity, cost, usage, and relationships.

- [Run security and compliance benchmarks](#run-security-and-compliance-benchmarks). Use pre-built benchmarks to assess how well your clouds comply with the standard frameworks including CIS, GDPR, NIST, PCI, SOC 2, and more.

- [Create your own dashboards and benchmarks](#create-your-own-dashboards-and-benchmarks). Build from scratch, using HCL in a live coding environment, or compose with the prebuilt dashboards and benchmarks.


## Pre-requisites

To get started, you will need to install Powerpipe, Steampipe, and the AWS plugin for Steampipe.

Powerpipe [does not require Steampipe](/docs/run#selecting-a-database); it can work with any PostgreSQL, MySQL, SQLite, or DuckDB database. [Mods](/docs/build) are written for a specific SQL syntax and database schema, however, and the examples in this article require a Steampipe database with the AWS plugin.  


First, install [Powerpipe](https://powerpipe.io/downloads).  

```bash+macos
brew install turbot/tap/powerpipe
```

```bash+linux 
sudo /bin/sh -c "$(curl -fsSL https://powerpipe.io/install/powerpipe.sh)"
```

Next, install [Steampipe](https://steampipe.io/downloads). 

```bash+macos
brew install turbot/tap/steampipe
```

```bash+linux
sudo /bin/sh -c "$(curl -fsSL https://steampipe.io/install/steampipe.sh)"
```

Now that Steampipe is installed, install & configure the [AWS plugin for Steampipe](https://hub.steampipe.io/plugins/turbot/aws)

```bash
steampipe plugin install aws
```

Out of the box, Steampipe will use the default AWS credentials from your credential file and/or environment variables; if you can run `aws ec2 describe-vpcs`, for example, then you should be able to run the examples. The AWS plugin documentation provides additional examples to [configure your credentials](https://hub.steampipe.io/plugins/turbot/aws#configuring-aws-credentials), and you can even configure Steampipe to query [multiple accounts](https://steampipe.io/docs#:~:text=steampipe%20to%20query-,multiple%20accounts,-and%20multiple%20regions) and [multiple regions](https://steampipe.io/docs#:~:text=multiple%20accounts%20and-,multiple%20regions).


Start the [Steampipe service](https://steampipe.io/docs/managing/service).  The Steampipe database needs to be running for Powerpipe to connect to it:

```bash
steampipe service start
```

At this point, Steampipe should be up and running.  You can confirm by running an ad-hoc query from Powerpipe:

```bash
powerpipe query run "select title from aws_account"
```


## Visualize cloud infrastructure

Powerpipe [dashboards](/docs/run/dashboard) provide rich visualizations of data. Dashboards are written in simple HCL, and packaged in mods.  It is simple to create your own, but there are also hundreds of dashboards available on the [Powerpipe Hub](https://hub.powerpipe.io/). 

Powerpipe always runs in the context of a [mod](/docs/build/).  A Powerpipe mod is a portable, versioned collection of related Powerpipe resources such as dashboards, benchmarks, queries, and controls defined in HCL, and distributed as simple text files.  Powerpipe loads the mod from the [mod location](/docs/run#mod-location) which defaults to the current directory.

Let's create a new directory for our mod:

```bash
mkdir learn_powerpipe
cd learn_powerpipe
```

Now initialize this mod:
```bash
powerpipe mod init
```

Powerpipe mods may depend on other mods.  When running Powerpipe from a mod, all dashboards in the mod and its direct dependencies will be available to run. Let's install the AWS Insights mod.  

```bash
powerpipe mod install github.com/turbot/steampipe-mod-aws-insights
```

Now start the Powerpipe server:
```
powerpipe server
```

Open `http://localhost:9033` in your web browser to view the list of available dashboards.

![](/images/docs/learn/aws_insights_dashboard_home.png)


You can click on a dashboard to run it.  Let's view the **AWS VPC Detail**.  You can scroll to find it, or use the **search** box at the top of the page.   

![](/images/docs/learn/vpc_detail.png)


Dashboards provide a wealth of information about your resources using charts, graphs, and tables.  You can [change the search path](/docs/run/dashboard/search-path), [take a snapshot](/docs/run/snapshots/interactive-snapshots) once the benchmark is complete, see details in the [panel view](/docs/run/dashboard#panel-view), and [download the results](/docs/run/dashboard/download) in a CSV file.

You can type in the search bar at the top of any page to navigate to another dashboard. Alternatively, you can click the Powerpipe logo in the top left to return to the home page. When you are finished, you can return to the terminal console and type `Ctrl+c` to stop the server.


## Run security and compliance benchmarks

Powerpipe benchmarks provide a mechanism for defining and running control frameworks such as CIS, NIST, HIPAA, etc, (as well as your own customized groups of controls) to assess your environment against security, compliance, operational, and cost controls.

Let's install the AWS Compliance mod and run some benchmarks. The AWS Compliance mod contains benchmarks and controls to evaluate your AWS account against various compliance frameworks, such as the CIS Amazon Web Services Foundations Benchmark.

```bash
powerpipe mod install github.com/turbot/steampipe-mod-aws-compliance
```

Let's run the CIS 3.0.0 benchmark:
```bash
powerpipe benchmark run aws_compliance.benchmark.cis_v300
```

The benchmark will run, reporting progress as it executes.  When it is complete, you will see the results printed to the console.  

![](/images/docs/learn/benchmark_run.webp)



The `powerpipe benchmark run` command provides a flexible interface for running benchmarks non-interactively, allowing you to [filter which controls to run]() and export the results in other formats (`csv`, `html`, `json`, `md`, `nunit3`, `pps` (snapshot), `asff`
).  

Benchmarks are also available as dashboards, allowing you to group and sort the results interactively.  Start the server (if it is not already running), and open the dashboard home in your browser (`http://localhost:9033`):

```bash
powerpipe server
```

Notice that the benchmarks from the AWS Compliance now appear in the list.  You can use the **Group by** button at the top of the page to **Group by: Mod**  or **Group by: Type** to find them more easily.

Click on **CIS v3.0.0** to run the CIS 3.0.0 benchmark.  You will see the results update as it runs, but you can interact with it even while the benchmark is running.

![](/images/docs/learn/benchmark_dashboard.png)


Click a section to expand it, and click again to contract it.  You can drill all the way down to the individual control results!

By default, the benchmark follows the grouping that is defined in the Benchmark HCL definition, but the dashboard display allows you to filter the results and customize the groupings if you prefer.  Hover over the benchmark and you will see action buttons appear at the top right of the page.  Click **Filter & Group** to open the **Filter & Group** panel.  For instance, to group by Service and Region, change the **Group** to group by `Control Tag = service`, then `Dimension = region` and `Result`.  Click apply.

![](/images/docs/learn/benchmark_by_service_region.png)

As with any dashboard, you can [change the search path](/docs/run/dashboard/search-path), [take a snapshot](/docs/run/snapshots/interactive-snapshots) once the benchmark is complete, see details in the [panel view](/docs/run/dashboard#panel-view), and [download the results](/docs/run/dashboard/download) in a CSV file.

## Create your own dashboards and benchmarks

The [Powerpipe Hub](https://hub.powerpipe.io/) contains hundreds of ready-made dashboards and benchmarks that you can simply install and run. But Powerpipe also makes it simple to [write your controls and benchmarks](/docs/build/writing-controls), and [build dashboards](/docs/build/writing-dashboards) to analyze your data and share with others! 


We've already [created our own mod](/docs/build/create-mod) and installed 2 dependency mods (AWS Insights and AWS Compliance).  Now let's add our own dashboard and benchmark.  Create a file named `learn.pp` in your mod's directory, and paste in the following HCL code:

```hcl

dashboard "learn_powerpipe_ex1" {
  title = "Learn Powerpipe"

  chart {
    type = "donut"
    width = 4
    title = "EC2 Instances by Region"
    sql = "select region, count(*) from aws_ec2_instance group by region"
  }

  chart {
    type = "donut"
    width = 4
    title = "EC2 Instances by Instance Type"
    sql = "select instance_type, count(*) from aws_ec2_instance group by instance_type"
  }

  chart {
    type = "donut"
    width = 4
    title = "EC2 Instances by State"
    sql = "select instance_state, count(*) from aws_ec2_instance group by instance_state"
  }
}

benchmark  "my_benchmark" {
  title    = "Custom compliance"
  children = [
    benchmark.my_iam_benchmark,
    benchmark.my_ec2_benchmark
  ]
}

benchmark  "my_iam_benchmark" {
  title    = "IAM"
  children = [
    aws_compliance.control.iam_group_user_role_no_inline_policies,
    aws_compliance.control.iam_group_not_empty,
    aws_compliance.control.iam_all_policy_no_service_wild_card
  ]
}

benchmark  "my_ec2_benchmark" {
  title    = "EC2"
  children = [
    aws_compliance.control.ec2_instance_in_vpc,
    aws_compliance.control.ec2_instance_iam_profile_attached,
    aws_compliance.control.ec2_instance_no_iam_passrole_and_lambda_invoke_function_access
  ]
}

```

If it is not already running, start the server and open the dashboard home in your browser (http://localhost:9033):

```bash
powerpipe server
```

Now use the button at the top to **Group by: Mod** and scroll down to your custom mod.  Notice that the dashboard and benchmark that you just created now appear. 

![](/images/docs/learn/custom_dash_list.png)

You can click and run them like any other dashboard!

![](/images/docs/learn/custom_dashboard.png)
