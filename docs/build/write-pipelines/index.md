---
#id: learn
#title: Learn Flowpipe
#sidebar_label: Learn Flowpipe
#slug: /
title: Write Pipelines
sidebar_label: Write Pipelines
---

# Write Pipelines

## Basics

A pipeline is composed of one or more steps:


```hcl
pipeline "my_job" {
    step "http" "my_http_step" {
        url = "https://example.com/my/webhook"
    }
}
```

Each step has a type and a name, and the arguments are dependent on the type.  The step types are sometimes referred to as "primitives".  The previous example included an `http` step, but there are others - `query`, `function`, `container`, etc.  You can even run other pipelines using a `pipeline` step.


You can run a pipeline manually from the command line:
```bash
flowpipe pipeline start pipeline.my_job
```

You can also define a trigger that will start the pipeline in response to an event.  For example, you can run it on a schedule with an `interval` trigger:

```hcl
trigger "interval" "my_hourly_trigger" {
    interval = "hourly"
    pipeline = pipeline.my_job
}

pipeline "my_job" {
    step "http" "my_http_step" {
        url = "https://example.com/my/webhook"
    }
}
```

Triggers, like steps, require 2 labels - a type and a name.  There are multiple trigger types -  `interval`, `http`, `cron`, `query`, etc.
