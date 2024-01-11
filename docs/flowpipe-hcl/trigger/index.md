---
title: trigger
sidebar_label: trigger
---


# trigger

Triggers are used to execute a pipeline when an event occurs. They are defined in the mod and are based on a schedule, webhook, or other event.

```hcl
trigger "http" "my_webhook" {
  pipeline = pipeline.my_pipeline
  args     = {
    event = self.request_body
  }                              
}
```


## Trigger Types

| Type            | Description
|-------------------|----------------
| [http](/docs/flowpipe-hcl/trigger/http)        | Create a webhook and initiate a pipeline when a request is made to the webhook.
| [schedule](/docs/flowpipe-hcl/trigger/schedule)| Runs a pipeline on schedule or at regular intervals.

<!--
| [query](/docs/flowpipe-hcl/trigger/query)      | Run a SQL query on a schedule and pass row changes as an input to the defined pipeline.

-->