---
title: pipeline
sidebar_label: pipeline
---

# pipeline

A `pipeline` is a sequence of steps to do work. Pipelines may define:
- [Steps](/docs/flowpipe-hcl/step/index) (`step`) to perform specific actions, such as running a query, making an http request, or running another pipeline. By default, steps will run in parallel but Flowpipe will automatically serialize steps based on implicit (HCL reference) or explicit (depends_on) dependencies.
- [Parameters](#parameters) (`param`) to allow you to pass arguments explicitly or from a `trigger`.
- [Outputs](#outputs) (`output`) to return data or status.

You can [run a pipeline manually](/docs/reference/cli/pipeline) from the command line or define a [trigger](/docs/flowpipe-hcl/trigger/index) that will start the pipeline in response to an event when running [`flowpipe server`](/docs/run/server). 


```hcl
pipeline "get_astronauts" {
  param "url" {
    type    = string
    default = "http://api.open-notify.org/astros"
  }

  step "http" "whos_in_space" {
      url    = param.url
      method = "get"
  }

  output "people_in_space" {
    value = step.http.whos_in_space.response_body.people[*].name
  }
}
```

## Arguments

| Argument        | Type    | Optional?   | Description
|-----------------|---------|-------------|-----------------
| `description`   | String  | Optional    | A description of the pipeline.
| `documentation` | String  | Optional | A markdown string containing a long form description, used as documentation for the mod on hub.steampipe.io. 
| `param`         | Block   | Optional    | A [param](#parameters) block that defines the parameters that can be passed into the pipeline. 
| `output`        | Block   | Optional    | one or more [output](#outputs) blocks to return values from the pipeline 
| `step`          | Block   | Optional    | one or more [steps](#steps) to run 
| `tags`          | Map     | Optional    | A map of key:value metadata for the benchmark, used to categorize, search, and filter.  The structure is up to the mod author and varies by benchmark and provider. 
| `title`         | String  | Optional    | Display title for the pipeline.


## Attributes (Read-Only)

| Attribute       | Type    |  Description
|-----------------|---------|---------------
| `errors`        | List    | List of [error objects](#errors-read-only) from the pipeline.
| `output`        | map     | map of [outputs](#outputs) defined and set by the pipeline.

----

## Parameters

One or more `param` blocks may optionally be used in a pipeline to define parameters that the pipeline accepts. 
```hcl
param "url" {
  type    = string
  default = "http://api.open-notify.org/astros"
}
```

You can then use the parameter value in the pipeline with the `param` object:
```hcl
step "http" "whos_in_space" {
    url    = param.url
    method = "get"
}
```

### Arguments


| Name          | Type    | Description
|---------------|---------|--------------------------
| `default`     | Any     | A value to use if no argument is passed for this parameter when the query is run.
| `description` | String  | A description of the parameter.
| `type`        | String   | The data type of the parameter: `string`, `number`, `bool`, `list`, `map`, `any` (default `any`). 

----

## Outputs

Output values make information from your pipeline run available to the caller. Output values are similar to return values in programming languages.

```hcl
output "people_in_space" {
  value = step.http.whos_in_space.response_body.people[*].name
}
```
### Arguments

| Name          | Type    | Description
|---------------|---------|--------------------------
| `description` | String  | A description of the output.
| `value`       | Any     | The value to export. (required)
