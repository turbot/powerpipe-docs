---
title: View Dashboards
sidebar_label: View Dashboards
---


# Powerpipe Dashboards


Powerpipe **dashboards** provide rich visualizations of data.  Dashboards are [written in simple HCL](/docs/powerpipe-hcl/dashboard), and packaged in [mods](/docs/build/).  It is simple to [create your own](/docs/build/writing-dashboards), but there are also hundreds of dashboards available on the [Powerpipe Hub](https://hub.powerpipe.io).  

You can start the dashboard server and view dashboards with the [powerpipe server](/docs/reference/cli/server) command.  Dashboards must be packaged in a mod, and Powerpipe [looks for dashboards in the current directory](/docs/run/index#mod-location) by default.  

To view the AWS Insights dashboards, for example, first clone the repo, then change to that directory, then run the `powerpipe server` command:

```bash
git clone https://github.com/turbot/powerpipe-mod-aws-insights.git
cd powerpipe-mod-aws-insights
powerpipe server
```

Powerpipe will start the dashboard server.  Open http://localhost:9194/ in your web browser to view the dashboards in the mod. 
<img src="/images/docs/dashboard_home.png" width="100%" />


The home page lists the available dashboards, and is searchable by title or tags.  By default, the dashboards are grouped by Category, but you may select another grouping if you prefer.


Click on the title of a report to view it.  For example, click the `AWS CloudTrail Trail Dashboard` to view it.

<img src="/images/docs/cloudtrail_dash_ex.png" width="100%" />

You can type in the search bar at the top of any page to navigate to another dashboard.  Alternately, you can click the Powerpipe logo in the top left to return to the home page.  When you are finished, you can return to the terminal console and type `Ctrl+c` to exit.


## Panel View

Most Powerpipe dashboard elements (chart, card, table, etc) support a **Panel View** that allows you to enlarge the element, inspect its definition, and easily export its data.  

To see the panel view, hover over the element and the expand button (4 opposing arrows) will appear at the top of the chart or table:   

<img src="/images/docs/cost_chart_with_expander.png" width="500px" />


Click the expand button to enter panel view.

The **Preview** pane displays the element in an enlarged view, allowing you to inspect detail that may be difficult to discern in the dashboard view.

<img src="/images/docs/cost_chart_preview.png" />


The **Definition** pane displays the HCL definition for the element. 

<img src="/images/docs/cost_chart_definition.png" />


The **Query** pane displays the SQL query used by the element.  You can cut and paste this query into a SQL query editor (including the Powerpipe query shell) to run or save it.

<img src="/images/docs/cost_chart_query.png" />


The **Data** pane displays a table of the results of the SQL query used by the element.  For a `table`, this will also include any hidden columns.  To download the data to CSV, click the **Download** button.

<img src="/images/docs/cost_chart_data.png" />

