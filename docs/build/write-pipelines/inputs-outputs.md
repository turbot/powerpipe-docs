---
title: Inputs and Outputs
sidebar_label: Inputs and Outputs
---
# Inputs and Outputs



A pipeline may define input parameters:

```hcl
pipeline "my_job" {
    param "url" {
        type    = "string"
        default = "https://example.com/my/webhook"
    }

    step "http" "my_step" {
        url = param.url
    }
}
```

A parameter may be optional:

```hcl
  param "unfurl_links" {
    type        = bool
    description = "Pass true to enable unfurling of primarily text-based content."
    optional    = true
  }
```

Mods can also define variables, and they are often used to set `param` defaults:

```hcl

variable "default_webhook" {
  default = "https://example.com/my/webhook"
}

pipeline "my_job" {
    param "url" {
        type    = "string"
        default = var.default_webhook
    }

    step "http" "first_step" {
        url = param.url
    }
}
```


If a param is not optional and has no default, you MUST pass it a value to run the pipeline:

```bash
flowpipe run pipeline.my_job --arg "url=https://example.com/mypath"
```

From a trigger or step, you can pass a value for that param using `args`:
```hcl
trigger "interval" "my_hourly_trigger" {
    interval = "hourly"
    pipeline = pipeline.my_job

    args = {
      url = "https://example.com/mypath"
    }
}

```


A pipeline may also define output values. The output values are printed to stdout when you run a pipeline:


```hcl
pipeline "get_astronauts" {
    step "http" "whos_in_space" {
        url    = "http://api.open-notify.org/astros"
        method = "get"
    }

    output "people_in_space" {
      value = step.http.whos_in_space.response_body.people
    }
}
```

Pipeline outputs also allow you to return a value to the calling pipeline when using nested pipelines:

```hcl
pipeline "get_astronauts" {
    step "http" "whos_in_space" {
        url    = "http://api.open-notify.org/astros"
        method = "get"
    }

    output "people" {
      value = step.http.whos_in_space.response_body.people
    }
}

pipeline "my_notification_pipeline" {

    step "pipeline" "get_astronauts" {
        pipeline = pipeline.get_astronauts
    }

    step "http" "notify_slack" {
        url    = "https://hooks.slack.com/services/T04THIS54LQ/B04ISH1B2GM/vIakTJfqFAKET7M14g5H32w8"
        method = "post"
        request_body = jsonencode({
          text = "people in space: ${join(", ", [for u in step.pipeline.get_astronauts.output.people : u.name])}"
        })
    }
}
```