---
title: Run Benchmarks
sidebar_label: Run Benchmarks
---

# Running Benchmarks

Powerpipe **controls** and **benchmarks** provide a generic mechanism for defining and running control frameworks such as CIS, NIST, HIPAA, etc, as well as your own customized groups of controls.

There are many control frameworks in existence today, and though they are all implemented with their own specific syntax and structure, they are generally organized in a defined, hierarchical structure, with a pass/fail type of [status](/docs/reference/mod-resources/control#control-statuses) for each item.  The control and benchmark resources allow Powerpipe to provide simplified, consistent mechanisms for defining, running, and returning output from these disparate frameworks.


Powerpipe benchmarks automatically appear as [dashboards](/docs/dashboard/overview) when you run `powerpipe dashboard` in the mod.  From the dashboard home, you can select any benchmark to run it and view it in an interactive HTML format.  You can even export the benchmark results as a CSV from the [panel view](/docs/dashboard/panel).

<img src="/images/reference_examples/benchmark_dashboard_view.png" />

<br />

You can also run controls and benchmarks in batch mode with the [powerpipe check](/docs/reference/cli/check) command.  The `powerpipe check` command provides options for selecting which controls to run, supports many output formats, and provides capabilities often required when using `powerpipe` in your scripts, pipelines, and other automation scenarios.  

To run every benchmark in the mod:

```bash
powerpipe check all
```

The console will show progress as its runs, and will print the results to the screen when it is complete:

<img src="/images/powerpipe-check-output-sample-1.png" width="100%" />



You can find controls and benchmarks in the [Powerpipe Mods](https://hub.powerpipe.io/mods) section of the [Powerpipe Hub](https://hub.powerpipe.io), or by searching [Github](https://github.com/topics/powerpipe-mod) directly.  

You can also [create your own controls and benchmarks](/docs/mods/writing-controls), and package them into a [mod](/docs/reference/mod-resources/overview).  



## More Examples

The [powerpipe check](reference/cli/check) command executes one or more Powerpipe benchmarks and controls.  You may specify one or more benchmarks or controls to run, or run powerpipe check all to run all 

You can run all controls in the workspace:
```bash
powerpipe check all 
```

Or only run a specific benchmark:
```bash
powerpipe check benchmark.cis_v130
```

Or run only specific controls:
```bash
powerpipe check control.cis_v130_1_4 control.cis_v130_2_1_1
```

Or only run controls with specific tags.  For example, to run the controls that have tags cis_level=1 and benchmark=cis:
```bash
powerpipe check all --tag cis_level=1 --tag cis=true
```

Usually, powerpipe mods use unqualified queries to "target" whichever connection is first in the [search path](/docs/managing/connections#setting-the-search-path), but you can specify a different path or prefix if you want:

```bash
powerpipe check all --search-path-prefix aws_connection_2
```


You can filter the controls to run using a where clause on the powerpipe_control reflection table.  
```bash
powerpipe check all --where "severity in ('critical', 'high')"
```



You can preview which controls with the `--dry-run` flag:
```bash
powerpipe check all --where "severity in ('critical', 'high')" --dry-run
```





-----


---
title: Formatting Output
sidebar_label: Formatting Output
---

# Formatting Output
By default, Powerpipe shows a progress bar, and produces colorized output to the console screen.  Powerpipe provides many options to control the output formatting.


By default, the console output uses 'dark mode' colors, but you can use 'light mode' if you prefer:
```bash
powerpipe check benchmark.cis_v130 --theme=light
```

If you run powerpipe from a CI tool or batch scheduler, you may want to use non-colorized output and disable the progress bar:
```bash
powerpipe check all --theme=plain --progress=false
```

Some benchmarks are quite verbose.  To show only the items that are in alarm or error, use `brief` output:
```bash
powerpipe check all --output=brief
```

You can also export the full output to JSON:
```bash
powerpipe check all --export=json
```

Or CSV:
```bash
powerpipe check all --export=csv
```

Or markdown:
```bash
powerpipe check all --export=md
```


Or html:
```bash
powerpipe check all --export=html
```


Or multiple formats:
```bash
powerpipe check all --export=csv --export=json --export=html
```

You can export to a filename of your choosing - powerpipe will infer the output type by the file extension:
```bash
powerpipe check all --export=output.csv --export=output.json --export=output.md
```

You can also send JSON output to stdout, if you want to redirect it to a file or pipe it to another program:
```bash
powerpipe check all --progress=false --output=json | jq
```

Powerpipe even allows you to [write your own control output templates!](develop/writing-control-output-templates).


