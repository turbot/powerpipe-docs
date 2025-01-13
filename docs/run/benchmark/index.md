---
title: Run Benchmarks
---
>[!NOTE]
> i think this page is generic to sp/tp and doesn't require specific subsections, but maybe add a tp example?

# Running Benchmarks

Powerpipe **controls** and **benchmarks** provide a generic mechanism for defining and running control [frameworks](/docs/powerpipe-hcl/control) such as CIS, NIST, HIPAA, etc. While such frameworks are implemented with their own specific syntax and structure, they are generally organized in a defined, hierarchical structure, with a pass/fail type of [status](/docs/powerpipe-hcl/control#control-statuses) for each item. Powerpipe mods abstract that pattern to provide a simple and consistent mechanisms for defining, running, and returning output from these disparate frameworks.


You can run controls and benchmarks in batch mode with the [powerpipe benchmark run](/docs/reference/cli/benchmark#powerpipe-benchmark-run) and [powerpipe control run](/docs/reference/cli/control#powerpipe-control-run) commands.  These commands provide options for selecting the controls to run, the output format, and other options you may need when using `powerpipe` in your scripts, pipelines, and other automation scenarios.

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


You can find controls and related benchmarks in the [Powerpipe Hub](https://hub.powerpipe.io), or by searching the [powerpipe-mod](https://github.com/topics/tailpipe-mod) topic on GitHub.

You can also [create your own controls and benchmarks](/docs/build/writing-controls) and package them into [mods](/docs/powerpipe-hcl/mod).

## Benchmarks as dashboards

Powerpipe benchmarks automatically appear as [dashboards](/docs/run/dashboard) when you run `powerpipe server` in the mod.  From the dashboard home, you can select any benchmark to run it and view it in an interactive HTML format. 

![](/images/docs/learn/benchmark_dashboard.png)

You can [change the search path](/docs/run/dashboard/search-path), [take a snapshot](/docs/run/snapshots/interactive-snapshots) once the benchmark is complete, see details in the [panel view](/docs/run/dashboard#panel-view), and [download the results](/docs/run/dashboard/download) to a CSV file.
<br />
