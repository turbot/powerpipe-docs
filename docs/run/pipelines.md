---
title: Run Pipelines
sidebar_label: Run Pipelines
---

# Run Pipelines

A Flowpipe [pipeline](/docs/flowpipe-hcl/step/pipeline) is a sequence of steps to do work.  Pipelines are distributed in [mods](/docs/build/index).  To run a pipeline, change to the mod directory and run `flowpipe pipeline run {pipeline name}`:

```bash
cd ~/src/flowpipe-samples/networking/ip_profiler
flowpipe pipeline run abuseipdb.pipeline.list_blacklisted_ip_addresses
```

By default, Flowpipe will load the mod from the current working directory, but you can load a mod from another directory and run its pipelines with the `--mod-location` argument:

```bash
flowpipe pipeline run abuseipdb.pipeline.list_blacklisted_ip_addresses --mod-location ~/src/flowpipe-samples/networking/ip_profiler
```

If you are running a [Flowpipe server](/docs/run/server) you can connect to the server instance and run one of its pipelines on the server with the `--host` argument:

```bash
flowpipe pipeline run abuseipdb.pipeline.list_blacklisted_ip_addresses --host local
```

By default, `flowpipe pipeline run` will stream event information to the console as the pipeline runs, and will wait for the pipeline to complete before exiting.  If you are running against a server instance (with `--host`) then you can choose to run the command asynchronously instead.  If you pass the `--detach` flag, Flowpipe will not wait for the pipeline to complete:
```bash
flowpipe pipeline run abuseipdb.pipeline.list_blacklisted_ip_addresses --host local --detach
```

If you have defined [workspaces](/docs/run/workspaces) you can use a workspace with the `--workspace` argument:

```bash
flowpipe pipeline run abuseipdb.pipeline.list_blacklisted_ip_addresses --workspace my_workspace
```


You can see what pipelines are available to run in the mod:

```bash
flowpipe pipeline list
```

The `flowpipe pipeline show` command will provide more detail about a single pipeline, including usage information:

```bash
flowpipe pipeline show reallyfreegeoip.pipeline.get_ip_geolocation
```

As with all Flowpipe commands, you can specify a different output format if you prefer:
```bash
flowpipe pipeline list --output json
```

You can pass arguments for any pipeline parameters that are defined with the `--arg` argument. 

```bash
flowpipe pipeline run reallyfreegeoip.pipeline.get_ip_geolocation --arg ip_address=4.2.2.1
```

Complex types like lists and maps will need to be quoted - single quotes usually work best so you don't have to escape the inner double quotes:

```bash
flowpipe pipeline run ip_profiler.pipeline.ip_profiler --arg ip_addresses='["4.2.2.1","4.2.2.2","4.2.2.3"]'
```

You may pass multiple `--arg` arguments as needed:

```bash
flowpipe pipeline run ip_profiler.pipeline.ip_profiler --arg ip_addresses='["4.2.2.1","4.2.2.2","4.2.2.3"]' --arg abuseipdb_max_age_in_days=90
```

Likewise, you can pass [variables](/docs/build/mod-variables) with one or more `--var` arguments:

```bash
flowpipe pipeline run aws.pipeline.describe_ec2_instances --var region=us-east-1 
```


Use `flowpipe process list` to see the details of current and prior pipeline runs:

```bash
flowpipe process list
```

Or use `flowpipe process show` to see the status of a single process, by its ID:

```bash
flowpipe process show exec_clppphbjtojfa9mh0tb0
```

If you started a pipeline run in a `flowpipe server` instance, you can even tail the details of the pipeline process as it runs:

```bash
flowpipe pipeline tail exec_cls9gp3jtoje6h9om3e0  --host local 
```
