---
title: View Dashboards
---

# Powerpipe Dashboards

Powerpipe **dashboards** provide rich visualizations of data.  Dashboards are [written in simple HCL](/docs/powerpipe-hcl/dashboard), and packaged in [mods](/docs/build/).  It is simple to [create your own](/docs/build/writing-dashboards), but there are also hundreds of dashboards available on the [Powerpipe Hub](https://hub.powerpipe.io).

To view dashboards, you must start the [dashboard server](/docs/run/server) from a [mod directory](/docs/run#mod-location).

## Viewing a dashboard

To view the AWS Insights dashboards, for example, [create a mod](/docs/build/create-mod) in an empty directory, [install the AWS Insights mod](/docs/build/mod-dependencies), and [start the server](/docs/run/server):

```bash
mkdir powerpipe_dashboards
cd powerpipe_dashboards
powerpipe mod init
powerpipe mod install https://github.com/turbot/steampipe-mod-aws-insights.git
powerpipe server
```

Powerpipe will start the dashboard server.  Open `http://localhost:9033/` in your web browser to view the dashboards in the mod. 

![](/dashboard_home.png)

The home page lists the available dashboards and is searchable by title or tags.  By default, the dashboards are grouped by **Category**, but you may select another grouping if you prefer.

Click on the title of a report to view it.  For example, click the `AWS CloudTrail Trail Dashboard` to view it.

![](/cloudtrail_dash_ex.png)

You can type in the search bar at the top of any page to navigate to another dashboard.  Alternatively, you can click the Powerpipe logo in the top left to return to the home page.  When you are finished, you can return to the terminal console and type `Ctrl+c` to exit.

## Viewing a benchmark

You call view benchmarks as dashboards, including those that present [controls](/docs/powerpipe-hcl/control) and those that present [detections](/docs/powerpipe-hcl/detection).
