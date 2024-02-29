---
title: Run Benchmarks
sidebar_label: Run Benchmarks
---

# Running Benchmarks

Powerpipe **controls** and **benchmarks** provide a generic mechanism for defining and running control frameworks such as CIS, NIST, HIPAA, etc, as well as your own customized groups of controls.

There are many control frameworks in existence today, and though they are all implemented with their own specific syntax and structure, they are generally organized in a defined, hierarchical structure, with a pass/fail type of [status](/docs/powerpipe-hcl/control#control-statuses) for each item.  The control and benchmark resources allow Powerpipe to provide simplified, consistent mechanisms for defining, running, and returning output from these disparate frameworks.

Powerpipe benchmarks automatically appear as [dashboards](/docs/run/dashboard) when you run `powerpipe server` in the mod.  From the dashboard home, you can select any benchmark to run it and view it in an interactive HTML format.  You can even export the benchmark results as a CSV from the [panel view](/docs/run/dashboard#panel-viewpanel).

<img src="/images/reference_examples/benchmark_dashboard_view.png" />

<br />

You can also run controls and benchmarks in batch mode with the [powerpipe benchmark run](/docs/reference/cli/benchmark#powerpipe-benchmark-run) and [powerpipe control run](/docs/reference/cli/control#powerpipe-control-run)commands.  These commands provide options for selecting the controls to run, the output format, and other options you may need when using `powerpipe` in your scripts, pipelines, and other automation scenarios.  

Powerpipe commands must be run in the contest of a mod, and are relative to the current directory.  You can pass the mod directory with the `--mod-location` argument, but its usually easier just to change to the mod directory:

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

The console will show progress as its runs, and will print the results to the screen when it is complete:

<img src="/images/powerpipe-check-output-sample-1.png" width="100%" />

You can find controls and benchmarks in the  [Powerpipe Hub](https://hub.powerpipe.io), or by searching [Github](https://github.com/topics/powerpipe-mod) directly.  

You can also [create your own controls and benchmarks](/docs/mods/writing-controls), and package them into a [mod](/docs/powerpipe-hcl/overview).  



## Running specific controls

While you will most commonly want to run all the controls in a benchmark, there are times when you want to only run single control, or filter the controls to run in a benchmark.

You can list the controls:
```bash
powerpipe control list
```

And run them by name, similar to benchmarks:
```bash
powerpipe control run my_control
```

Unlike benchmarks, some controls may define parameters.  You can pass values for them using one or more `--arg` argument:
```bash
powerpipe control run my_control_with_params --arg my_simple_arg='this is a string' --arg my_list_arg='["item 1","item 2"]'
```

If the parameters are unnamed, you can pass values for them by index:
```bash
powerpipe control run my_control_with_params --arg 1='this is a string' --arg 2='["item 1","item 2"]'
```


## Filtering Benchmarks

You can pre-filter a benchmark to run a subset of the controls. Most benchmarks are arranged hierarchically.  If you want to view all of the benchmarks, including the nested ones,  pass `--all` to the `powerpipe benchmark list` command:

```bash
powerpipe benchmark list --all
```

You can run any of these nested benchmarks:
```bash
powerpipe benchmark run my_benchmark_section_1
```

You can also filter the benchmark to only run controls with specific tags.  For example, to run the controls that have tags `cis_level=1` and `benchmark=cis`:
```bash
powerpipe benchmark run cis_v150 --tag cis_level=1 --tag cis=true
```


***[TODO show how to inspect what fields are availble with powerpipe control show controlname --output json]***

You can also filter the controls to run using a where clause on the properties of the benchmark.  
```bash
powerpipe benchmark run cis_v150 --where "severity in ('critical', 'high')"
```


You can preview which controls will run with the `--dry-run` flag:
```bash
powerpipe benchmark run cis_v150 --where "severity in ('critical', 'high')" --dry-run
```


## Targeting specific connections (Postgres only)

A PostgreSQL database contains one or more [schemas](https://www.postgresql.org/docs/current/ddl-schemas.html). A schema is a namespaced collection of named objects, like tables, functions, and views.   When writing a query, you may qualify the table name with the schema name (e.g. `select * from schema_name.table_name`) or use an unqualified name (`select * from table_name`).  When using unqualified names, the system determines which table is meant by following a [search path](https://www.postgresql.org/docs/current/ddl-schemas.html#DDL-SCHEMAS-PATH); the first matching table in the search path is taken to be the one wanted. 

Usually, Powerpipe mods use unqualified queries to "target" whichever connection is first in the [search path](https://steampipe.io/docs/guides/search-path), but you can specify a different path if you want:

```bash
powerpipe benchmark run cis_v150 --search-path aws_connection_2,github,slack
```

The `--search-path` argument will replace the entire search path.  Often you just want to move a single connection to the front of the path:

```bash
powerpipe benchmark run cis_v150 --search-path-prefix aws_connection_2
```

[Steampipe](https://steampipe.io) creates a schema for each connection and aggregator, and understanding how the search path works is important when using Steampipe and Powerpipe. See the [Using search_path to target connections and aggregators](https://steampipe.io/docs/guides/search-path) guide for more information.


## Selecting a database
By default, Powerpipe will run against a local Steampipe instance.  This is because the default workspace database is set to `postgres://steampipe@localhost:9193/steampipe`.  

You may instead run a benchmark or control against a specific database by passing the `--database` argument:
```bash
powerpipe benchmark run cis_v120 --database  postgres://myusername:passworrd@mydbserver.mydomain.com:9193/steampipe
```

Or setting the POWERPIPE_DATABASE environment variable:

```bash
export POWERPIPE_DATABASE=postgres://myusername:passworrd@mydbserver.mydomain.com:9193/steampipe
powerpipe benchmark run cis_v120
```

You can also set it in a [workspace](/docs/run/workspaces) and then pass the workspace name to the command:
```bash
powerpipe benchmark run cis_v120 --workspace my_workspace
```

You can even change the default by setting it in your `default` workspace.

If you use [Turbot Pipes](http://pipes.turbot.com), you can run a benchmark against the pipes workspace by name (you will need to [login](/docs/reference/cli/login) first):
```bash
powerpipe benchmark run cis_v120 --workspace acme/anvils
```


## Taking Snapshots

Powerpipe allows you 
Run a benchmark and upload a snapshot with `workspace` visibility in your user workspace.
```bash
powerpipe benchmark run cis_v120 --snapshot 
```


Run a benchmark and upload a snapshot with `anyone_with_link` visibility in your user workspace.
```bash
powerpipe benchmark run cis_v120 --share 
```


Run a benchmark and upload a snapshot with `anyone_with_link` visibility to specific workspace.
```bash
powerpipe benchmark run cis_v120 --share --snapshot-location vandelay-industries/latex
```

Run a benchmark, upload a snapshot with `workspace` visibility in your user workspace, and tag the snapshot:
```bash
powerpipe benchmark run cis_v120 --snapshot --snapshot-tag env=local 
```




## Formatting Output
By default, Powerpipe shows a progress bar, and produces colorized output to the console screen.  Powerpipe provides many options to control the output formatting.

<!--
By default, the console output uses 'dark mode' colors, but you can use 'light mode' if you prefer:
```bash
powerpipe benchmark run benchmark.cis_v130 --theme=light
```

-->

If you run powerpipe from a CI tool or batch scheduler, you may want to use non-colorized output and disable the progress bar:
```bash
powerpipe benchmark run cis_v150 --theme=plain --progress=false
```

Some benchmarks are quite verbose.  To show only the items that are in alarm or error, use `brief` output:
```bash
powerpipe benchmark run cis_v150 --output=brief
```

You can also export the full output to JSON:
```bash
powerpipe benchmark run cis_v150 --export=json
```

Or CSV:
```bash
powerpipe benchmark run cis_v150 --export=csv
```

Or markdown:
```bash
powerpipe benchmark run cis_v150 --export=md
```


Or html:
```bash
powerpipe benchmark run cis_v150 --export=html
```


Or export to multiple formats from a single run:
```bash
powerpipe benchmark run cis_v150 --export=csv --export=json --export=html
```

You can export to a filename of your choosing - Powerpipe will infer the output type by the file extension:
```bash
powerpipe benchmark run cis_v150 --export=output.csv --export=output.json --export=output.md
```

You can also send JSON output to stdout, if you want to redirect it to a file or pipe it to another program:
```bash
powerpipe benchmark run cis_v150 --progress=false --output=json | jq
```


***[TODO:  where to put this page....]***
Powerpipe even allows you to [write your own control output templates!](develop/writing-control-output-templates).



