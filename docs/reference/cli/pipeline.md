---
title: flowpipe pipeline
sidebar_label: flowpipe pipeline
---

# flowpipe pipeline

List, view, and run Flowpipe pipelines.

## Usage
```bash
flowpipe pipeline list [args]
flowpipe pipeline show pipeline_name [args]
flowpipe pipeline run pipeline_name [args]
```



## Sub-Commands

| Command | Description
|-|-
| `list` | List pipelines from the current mod and its direct dependents.
| `run`  | Run a pipeline from the current mod or its direct dependents or from a Flowpipe server instance.
| `show` | Show details of a pipeline from the current mod or its direct dependents.

## Arguments

| Flag | Applies to | Description
|-|-|-
| `--arg string=string` | `run` | Specify the value of a pipeline argument. Multiple `--arg` arguments may be passed.
| `--detach`   | `run` | Start the pipeline and return immediately.  By default, `flowpipe pipeline run` will run the pipeline and wait for the results. You may only use `--detach` when running a pipeline from a server instance (by specifying `--host`, for example).
| `--var string=string` | `run`| Specify the value of a variable.  Multiple `--var` arguments may be passed. 
| `--var-file strings`| `run`| Specify an .fpvar file containing variable values.
| `--verbose`   | `run` | View detailed event information, including step arguments and attributes.


## Examples

List pipelines:

```bash
flowpipe pipeline list
```

List pipelines in JSON format:

```bash
flowpipe pipeline list --output json
```

View pipeline details:

```bash
flowpipe pipeline show my_pipeline
```

Run a pipeline in the current mod:

```bash
flowpipe pipeline run my_pipeline
```

Run a pipeline with verbose output:

```bash
flowpipe pipeline run my_pipeline --verbose
```

Run a pipeline in a direct dependency mod:
```bash
flowpipe pipeline run my_dependency_mod.pipeline.my_pipeline
```

Run a pipeline and pass parameters:

```bash
flowpipe pipeline run my_pipeline --arg my_string_param="my name" --arg 'my_list_param=["Owner","Application","Environment"]'
```


Run a pipeline from the local server instance (started with `flowpipe server`):
```bash
flowpipe pipeline run my_pipe --host local
```
or
```bash
flowpipe pipeline run my_pipe --host http://localhost:7103
```

Start a pipeline from the local server instance and run it asynchronously:
```bash
flowpipe pipeline run my_pipe --host local --detach
```


Run a pipeline from a remote server instance:
```bash
flowpipe pipeline run my_pipe --host http://flowpipe.mycompany.com:7103
```

Run a pipeline from a remote server instance, do not verify the TLS certificate:
```bash
flowpipe pipeline run my_pipe --host http://flowpipe.mycompany.com:7103  --insecure
```

Run a pipeline using settings from a workspace:
```bash
flowpipe pipeline run my_pipeline --workspace my_workspace
```

