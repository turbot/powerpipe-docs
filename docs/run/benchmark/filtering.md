---
title: Filtering Controls
---

# Filtering Controls

While you will most commonly want to run all the controls in a benchmark, there are times when you want to only run single control, or filter the controls to run in a benchmark.


## Filtering by Control Tags

You can filter controls by the control tags.  For example, to run the controls in the `cis_v150` benchmark that have tags `cis_level=1` and `benchmark=cis`:
```bash
powerpipe benchmark run cis_v150 --tag cis_level=1 --tag cis=true
```

This filter works with `all` as well:
```bash
powerpipe benchmark run all --tag cis_level=1 --tag cis=true
```


## Filtering with `--where`

You can also filter the controls to run using a `where` clause on the properties of the benchmark. 

```bash
powerpipe benchmark run all --where "severity in ('critical', 'high')"
```


You can preview which controls will run with the `--dry-run` flag:
```bash
powerpipe benchmark run cis_v150 --where "severity in ('critical', 'high')" --dry-run
```



## Running specific controls
You can run a single control at a time with the `powerpipe control` command.

You can list the controls:
```bash
powerpipe control list
```

And run them by name, similar to benchmarks:
```bash
powerpipe control run my_control
```

Unlike benchmarks, some controls may define parameters.  You can pass values for them using one or more `--arg` arguments:
```bash
powerpipe control run my_control_with_params --arg my_simple_arg='this is a string' --arg my_list_arg='["item 1","item 2"]'
```

If the parameters are unnamed, you can pass values for them without specifying a name.  The args will be passed to the query in order - the first `--arg` as `$1`, the second as `$2`, etc.:
```bash
powerpipe control run my_control_with_params --arg 'this is a string' --arg '["item 1","item 2"]'
```