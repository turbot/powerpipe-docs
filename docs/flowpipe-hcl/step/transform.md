---
title: transform
sidebar_label: transform
---


# transform

A transform step can be used to merge, filter, or reformat data using HCL functions.  While you can transform the data "inline", it is often useful to:
  - transform the data once and use the transformed data in multiple places
  - transform the data through multiple distinct steps to makes it easier to troubleshoot / maintain
  - perform conditional logic as part of the data transformation

```hcl
pipeline "transform_example" {

  step "http" "whos_in_space" {
    url    = "http://api.open-notify.org/astros"
    method = "get"
  }

  step "transform" "get_spacecraft" {
    value = distinct(step.http.whos_in_space.response_body.people[*].craft)
  }

  step "transform" "get_astros" {
    value = distinct(step.http.whos_in_space.response_body.people[*].name)
  }

  output "astronauts" {
    value = step.transform.get_astros.value
  }
  output "spacecraft" {
    value = step.transform.get_spacecraft.value
  }
  
}
```

## Arguments

| Argument        | Type    | Optional?  | Description
|-----------------|---------|------------|-----------------
| `value`        | Any     | Required | The step value.  Usually, this is the field where you will transform the data


This step also supports the [common step arguments](/docs/flowpipe-hcl/step/index#common-step-arguments) and [attributes](/docs/flowpipe-hcl/step/index#common-step-attributes-read-only).
