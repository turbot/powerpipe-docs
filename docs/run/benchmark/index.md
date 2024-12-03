---
title: Run Benchmarks
---

# Running Benchmarks

Powerpipe **controls**, **detections**, and **benchmarks** provide a generic mechanism for defining and running control frameworks[/docs/powerpipe-hcl/control] such as CIS, NIST, HIPAA, etc, or [detections](/docs/powerpipe-hcl/detection) as well as your own customized groups of controls or detect

There are many control frameworks in existence today, and though they are all implemented with their own specific syntax and structure, they are generally organized in a defined, hierarchical structure, with a pass/fail type of [status](/docs/powerpipe-hcl/control#control-statuses) for each item.  The control and benchmark resources allow Powerpipe to provide simplified, consistent mechanisms for defining, running, and returning output from these disparate frameworks.

For detections we support (https://attack.mitre.org)[MITRE ATT&CK®] framework.

Powerpipe benchmarks automatically appear as [dashboards](/docs/run/dashboard) when you run `powerpipe server` in the mod.  From the dashboard home, you can select any benchmark to run it and view it in an interactive HTML format. 

![](/images/docs/learn/benchmark_dashboard.png)

As with any dashboard, you can [change the search path](/docs/run/dashboard/search-path), [take a snapshot](/docs/run/snapshots/interactive-snapshots) once the benchmark is complete, see details in the [panel view](/docs/run/dashboard#panel-view), and [download the results](/docs/run/dashboard/download) in a CSV file!
<br />

You can also run controls, detections, and benchmarks in batch mode with the [powerpipe benchmark run](/docs/reference/cli/benchmark#powerpipe-benchmark-run) and [powerpipe control run](/docs/reference/cli/control#powerpipe-control-run) commands.  These commands provide options for selecting the controls to run, the output format, and other options you may need when using `powerpipe` in your scripts, pipelines, and other automation scenarios.

Powerpipe commands must be run in the contest of a mod, and are relative to the current directory.  You can pass the mod directory with the `--mod-location` argument, but it's usually easier just to change to the mod directory:

```bash
cd ~/my_powerpipe_mod
```

You can list the runnable benchmarks:
```bash
powerpipe benchmark list
```

To run a benchmark, run it by name:
```bash
powerpipe benchmark run all_benchmarks
```

You can run benchmarks in any direct mod dependency, but you have to fully qualify them:
```bash
powerpipe benchmark run my_other_mod.benchmark.my_benchmark
```

You can even run all benchmarks in the mod with the keyword `all`.  Note that `powerpipe benchmark run all` will not run benchmarks in the dependencies:
```bash
powerpipe benchmark run all
```


The console will show progress as it runs, and will print the results to the screen when it is complete:

![](/images/docs/learn/benchmark_run.webp)


You can find controls and related benchmarks in the  [Powerpipe Hub](https://hub.powerpipe.io), or by searching [Github](https://github.com/topics/powerpipe-mod) directly.

You can find detections and related benchmarks in the  [Tailpipe Hub](https://hub.tailpipe.io), or by searching [Github](https://github.com/topics/tailpipe-mod) directly.

You can also [create your own controls and benchmarks](/docs/build/writing-controls), and package them into a [mod](/docs/powerpipe-hcl/mod).

And you can [create your own detections and benchmarks](/docs/build/writing-detections).

You can package controls or detections into a   xa [mod](/docs/powerpipe-hcl/mod).

