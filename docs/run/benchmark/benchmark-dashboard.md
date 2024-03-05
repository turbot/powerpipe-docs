---
title: Dashboard View
sidebar_label: Dashboard View
---


# Benchmark Dashboard View

The `powerpipe benchmark run` command provides a flexible interface for running benchmarks non-interactively, allowing you to [filter which controls to run](/docs/run/benchmark/filtering) and export the results in other formats (`csv`, `html`, `json`, `md`, `nunit3`, `pps` (snapshot), `asff`
).  

Benchmarks are also available as [dashboards](/docs/run/dashboard), providing a richer interactive experience.  When running a benchmark from the [dashboard server](/docs/run/server), you can explore the results interactively, and even group and filter the results in different ways.  

When you run `powerpipe server` from a mod that includes benchmarks, they will appear in the list of dashboards.  Simply click to run them, like any other dashboard.  You will see the results update as the benchmark runs, but you can interact with it even while the benchmark is running.

![](/images/docs/learn/benchmark_dashboard.png)


## Filtering & Grouping
By default, the benchmark follows the grouping that is defined in the Benchmark HCL definition, but the dashboard display allows you to filter the results and customize the groupings if you prefer.  Hover over the benchmark and you will see action buttons appear at the top right of the page.  Click **Filter & Group** to open the **Filter & Group** panel.  For instance, to group by Service and Region, change the **Group** to group by `Control Tag = service`, then `Dimension = region` and `Result`.  Click apply.

![](/images/docs/learn/benchmark_by_service_region.png)

As with any dashboard, you can [change the search path](/docs/run/dashboard/search-path), [take a snapshot](/docs/run/snapshots/interactive-snapshots) once the benchmark is complete, see details in the [panel view](/docs/run/dashboard#panel-view), and [download the results](/docs/run/dashboard/download) in a CSV file!

