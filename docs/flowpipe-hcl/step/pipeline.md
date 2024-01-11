---
title: pipeline
sidebar_label: pipeline
---

# pipeline

A pipeline step executes another Flowpipe pipeline from the same mod or its direct dependencies:

```hcl
pipeline "my_pipeline" {

  step "pipeline" "post_message" {
    pipeline = slack.pipeline.chat_post_message
    args = {
      token   = "xoxp-YOUR_TOKEN_HERE"
      channel = "#general"
      message = "This is a test message"
    }
  }
}
```

## Arguments

| Argument        | Type    | Optional?  | Description
|-----------------|---------|------------|-----------------
| `pipeline`      | Pipeline Reference | Required | A reference to a `pipeline` resource to run in this step.  
| `args`	        | Map	    | Optional	  | A map of arguments to pass to the pipeline.

This step also supports the [common step arguments](/docs/flowpipe-hcl/step/index#common-step-arguments) and [attributes](/docs/flowpipe-hcl/step/index#common-step-attributes-read-only).


## Attributes (Read-Only)

| Attribute       | Type    |  Description
|-----------------|---------|-----------------
| `errors`        | List    | List of [errors](/docs/flowpipe-hcl/pipeline#errors-attribute-read-only) from the child pipeline.
| `output`        | Map     | A map of the `output` elements from the child pipeline merged with any [custom step outputs](/docs/flowpipe-hcl/pipeline#output--attribute-read-only) defined in [output blocks](/docs/flowpipe-hcl/pipeline#output-block)
