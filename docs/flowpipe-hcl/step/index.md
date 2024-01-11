---
title: step
sidebar_label: step
---


# step

A pipeline is composed of one or more steps. Each step has a type and a name, and the arguments are dependent on the type. The step types are sometimes referred to as "primitives". 

Each step type has its own distinct set of attributes, though there are some [common step arguments](#common-step-arguments) and [attributes](#common-step-attributes-read-only) that all steps implement.

## Step Types

| Type            | Description
|-------------------|----------------
| [container](/docs/flowpipe-hcl/step/container)    | Run a Docker container.
| [email](/docs/flowpipe-hcl/step/email)            | Send an email.
| [function](/docs/flowpipe-hcl/step/function)      | Run an AWS Lambda-compatible function.
| [http](/docs/flowpipe-hcl/step/http)              | Make an HTTP request.
| [pipeline](/docs/flowpipe-hcl/step/pipeline) | Run another Flowpipe pipeline.
| [query](/docs/flowpipe-hcl/step/query)            | Run a SQL query.
| [sleep](/docs/flowpipe-hcl/step/sleep)            | Wait for a defined time period.
| [transform](/docs/flowpipe-hcl/step/transform)    | Use HCL functions to transform data .

<!--
| [input](/docs/flowpipe-hcl/step/input)            | Request human input.

-->

## Common Step Arguments

| Argument        | Type      | Optional?   | Description
|-----------------|-----------|-------------|-----------------
| `depends_on`    | List of Steps | Optional | A list of steps that this step [depends on](#depends_on).
| `description`   | String  | Optional    | A description of the step.
| `error`         | Block   | Optional | An [error block](#error) to handle errors from the step.
| `for_each`      | Map or List | Optional | A map or list used as a [step iterator](#for_each).  A step instance will be created for each item in the map or list.
| `if`            | Condition| Optional  | An [if condition](#if) to evaluate to determine whether to run this step.
| `loop`        | Block   | Optional | A [loop block](#loop) to run the step in a sequential loop.
| `output`        | Block   | Optional | One or more [output blocks](#output) to return custom values from the step.
| `retry`        | Block   | Optional | A [retry block](#retry) to retry the step when an error occurs.
| `throw`        | Block   | Optional | One or more [throw blocks](#throw) to raise an error from the step.
| `timeout`       | Number or String| Optional	  | [Amount of time](#timeout) this step has to run before an error is raised.
| `title`         | String | Optional | A display title for the step.

###  depends_on

The `depends_on` argument allows you to define explicit dependencies to make steps run in a specific order.  Note that Flowpipe will create [implicit dependencies](/docs/build/write-pipelines/control-flow) based on references to other steps so you only need `depends_on` when you don't have an implicit reference.  

The `depends_on` argument accepts a list of steps that this step depends on:
```hcl
step "sleep" "sleep_10_seconds" {
  depends_on = [ step.http.http_1 ]
  duration   = "10s"
}
``` 


### for_each

The `for_each` argument is used to [run a step in a parallel loop](/docs/build/write-pipelines/iteration#for_each).  This argument accepts a map or list used as a step iterator.  A step instance will be created for each item in the map or list:

```hcl
step "http" "add_a_user" {
    for_each = ["Jerry","Elaine", "Newman"]
    url      = "https://myapi.local/api/v1/user"
    method   = "post"

    request_body = jsonencode({
        user_name = "${each.value}" 
    })
} 
```


### if

The `if` argument accepts a [condition to evaluate to determine whether to run this step](/docs/build/write-pipelines/conditionals).  The step will only run when `if` evaluates to true:

```hcl
step "email" "send_it" {
  if      = step.pipeline.order.output.order_count > 0
  to      = ["darin@kramerica.com"]
  from    = "elaine@jpetermancatalog.com"
  host    = "smtp.example.com"
  subject = "Order Shipped"
  body    = "Your order has shipped"
}
```

### timeout

Most steps accept a `timeout` argument which specifies the amount of time to wait for the step to complete before raising an error.

The `timeout` argument may be an integer or a [Go duration string](https://pkg.go.dev/time#Duration).  If the duration is an integer, it will be interpreted as the number of milliseconds:


```hcl
step "http" "whos_in_space" {
  url    = "http://api.open-notify.org/astros"
  method = "get"
  timeout = 5000
}
```

You may instead pass a string that specifies the number and type of units.  Valid time units are `ns`, `us` (or `µs`), `ms`, `s`, `m`, `h`. Note that the granularity varies by step type, and fractional amounts will be rounded up to the appropriate granularity.  For instance, The `timeout` for a `container` has a granularity of 1 second, so if you set the timeout to `500ms` it will be rounded up to 1 second.

```hcl
step "http" "whos_in_space" {
  url    = "http://api.open-notify.org/astros"
  method = "get"
  timeout = "5s"
}
```

You can even include multiple units:

```hcl
step "http" "whos_in_space" {
  url    = "http://api.open-notify.org/astros"
  method = "get"
  timeout = "1m5s"
}
```


### error

By default, all errors are fatal and are not retried - When a step encounters an error, it causes the step the fail. A failed step results in a failed pipeline - Any step instances that are already running will complete (but will not be retried) but then the pipeline will stop with a failed status.

The `error` block allows you to [ignore the error](/docs/build/write-pipelines/errors#error) and continue execution. You can then "handle" the error in subsequent steps.

```hcl
step "http" "my_request" {
  url           = "https://myapi.local/subscribe"
  method        = "post"
  body          = jsonencode({
    name = param.subscriber
  })

  error {
    ignore = true
  }
}

step "email" "send_it" {
  to      = param.subscriber
  subject = "You have been subscribed"
  body    = step.http.my_request.response_body
  if      = !is_error(step.http.my_request)
}
```

#### Arguments

| Argument        | Type        | Optional?  | Description
|-----------------|-------------|------------|-----------------
| `if`            | Boolean     | Optional   | A condition to evaluate to determine whether to evaluate this error block.  The `error` block will only be evaluated when the `if` condition evaluates to `true`.
| `ignore`        | Boolean     | Optional   | If `true`, the error will be ignored.



### loop
The `loop` block will [run a step in a sequential loop](/docs/build/write-pipelines/iteration#loop), changing the arguments with each iteration. This is useful for handling HTTP pagination, for example. 

```hcl
step "http" "list_workspaces" {
  url    = "https://latestpipe.turbot.io/api/v1/org/latesttank/workspace/?limit=3"
  method = "get"

  request_headers = {
    Content-Type  = "application/json"
    Authorization = "Bearer ${param.pipes_token}"
  }

  loop {
    until = result.response_body.next_token == null
    url = "https://latestpipe.turbot.io/api/v1/org/latesttank/workspace/?limit=3&next_token=${result.response_body.next_token}"
  }

}
```

The loop is evaluated last after the step instance has executed and all retries have completed. You can use the special value `result` to evaluate the attributes of the completed step instance. `result` is essentially a self-reference to "this" step after it has run (e.g. the attributes are populated).

You can also use the special attribute called `loop` inside the block. `loop` has a single attribute `index` which is the (zero-based) index of the loop count.

#### Arguments

| Argument        | Type        | Optional?  | Description
|-----------------|-------------|------------|-----------------
| `until`         | Boolean     | Optional   | A condition to evaluate to determine whether to run this step again.  The step will loop until this condition is `true`. 
| `{any argument}`| any         | Optional   |  A step argument to override for the next iteration. When the step runs again due to the loop, it inherits all of the arguments from the step, but you can override them inside the loop block if desired, allowing you to pass data from the step execution into the next step execution


### retry

The `retry` block allows you to [retry the step](/docs/build/write-pipelines/errors#retry) when an error occurs.

```hcl
step "http" "my_request" {
  url           = "https://myapi.local/subscribe"
  method        = "post"
  body          = jsonencode({
    name = param.subscriber
  })

  retry {
    max_attempts = 5
    strategy     = "exponential"
    min_interval = 100
    max_interval = 10000
  }
}
```

#### Arguments

| Argument        | Default        | Optional?  | Description
|-----------------|-------------|------------|-----------------
| `if`            | `true`      | Optional   | A condition to evaluate to determine whether to retry the step.  The step will be retried only if the `if` condition evaluates to `true`.
| `max_attempts`  | `3`         | Optional   | Specifies the maximum number attempts to run the step.
| `strategy`      | `constant`  | Optional   | The backoff strategy. One of `exponential`, `linear`, `constant`. 
| `min_interval`  | `1000`      | Optional   | The first interval between retries, in milliseconds. If the strategy is exponential or linear, subsequent intervals will be scaled based on this value.
| `max_interval`  | `10000`     | Optional   | The maximum interval between retries, in milliseconds.


### throw

You can also [explicitly raise an exception](/docs/build/write-pipelines/errors#throw) with a `throw` block to raise an error when the step would usually succeed. You can include as many `throw` blocks as you want. Thrown errors are not retried, though they can be ignored.


```hcl
step "http" "my_request" {
  url           = "https://myapi.local/subscribe"
  method        = "post"
  body          = jsonencode({
    name = param.subscriber
  })

  throw {
    if      = length(result.response_body.errors) > 1
    message = result.response_body.errors[0]
  }

}
```

#### Arguments

| Argument        | Type        | Optional?  | Description
|-----------------|-------------|------------|-----------------
| `message`       | String      | Required   | The error string for the thrown error.
| `if`            | Boolean     | Optional   | A condition to evaluate to determine whether to throw an error.  The step will only raise an error when the `if` condition evaluates to `true`.


### output
You may include one or more [output](#output) blocks to arbitrary values in the `output` attribute of the step. 

```hcl

pipeline "get_astros" {

  step "http" "whos_in_space" {
    url    = "http://api.open-notify.org/astros"
    method = "get"
  }

  step "transform" "parse_astros" {
    value = step.http.whos_in_space.response_body.people

    output "astronauts" {
      value = step.http.whos_in_space.response_body.people[*].name
    }

    output "spacecraft" {
      value = distinct(step.http.whos_in_space.response_body.people[*].craft)
    }
  }

  output "people_in_space" {
    value = step.transform.parse_astros.output.astronauts
  }

  output "ships_in_space" {
    value = step.transform.parse_astros.output.spacecraft
  }
}
```

#### Arguments

| Argument        | Type    | Optional?  | Description
|-----------------|---------|------------|-----------------
| `value`         | Any     | Required | The output value.  



## Common Step Attributes (Read-Only)
| Attribute       | Type    |  Description
|-----------------|---------|-----------------
| `errors`        | List    | List of [errors](#errors-read-only) from the step
| `output`        | Map     | A [map of the step outputs](#output-read-only) defined in [output blocks](#output)

### errors (Read-Only)

Each step has an [`errors` attribute](/docs/build/write-pipelines/errors#the-errors-attribute) that contains a list of errors that occurred. [Unhandled errors](/docs/build/write-pipelines/errors#handling-errors) will cause the pipeline run to fail and will be returned in the pipeline `errors` list.  

To simplify common error handling cases, Flowpipe provides some helper functions:
- `is_error`:  Given a reference to a step, `is_error` returns a boolean `true` if there are 1 or more errors, or false if there are no errors. `is_error(step.http.my_request)` is equivalent to `length(step.http.my_request.errors) > 0`
-  `error_message`:  Given a reference to a step, `error_message` will return a string containing the first error message, if any. If there are no errors, then it will return an empty string.  This is useful for simple step primitives. 



### output (Read-Only)
You can access [custom step outputs](#output-block) using the `output` attribute of a step.  The `output` attribute contains a map of outputs for the step, with an entry for each `output` block.




<!--


| Argument        | Type    | Optional?  | Description
|-----------------|---------|------------|-----------------
| `duration`      | String or Number | Required | The amount of time to sleep as an integer or a [duration string](#duration-strings).  If the duration is an integer, it is interpreted as the number of milliseconds.  
| `depends_on`    | List of Steps | Optional | A list of steps that this step [depends on](/docs/flowpipe-hcl/pipeline#depends_on-argument).
| `description`   | String  | Optional    | A description of the step.
| `error`         | Block   | Optional | An [error block](/docs/flowpipe-hcl/pipeline#error-block) to handle errors from the step.
| `for_each`      | Map or List | Optional | A map or list used as a [step iterator](/docs/flowpipe-hcl/pipeline#for_each-argument).  A step instance will be created for each item in the map or list.
| `if`            | Condition| Optional  | An [if condition](/docs/flowpipe-hcl/pipeline#if-argument) to evaluate to determine whether to run this step.
| `loop`        | Block   | Optional | A [loop block](/docs/flowpipe-hcl/pipeline#loop-block) to run the step in a sequential loop.
| `output`        | Block   | Optional | One or more [output blocks](/docs/flowpipe-hcl/pipeline#output-block) to return custom values from the step.
| `retry`        | Block   | Optional | A [retry block](/docs/flowpipe-hcl/pipeline#retry-block) to retry the step when an error occurs.
| `throw`        | Block   | Optional | One or more [throw blocks](/docs/flowpipe-hcl/pipeline#throw-block) to raise an error from the step.
| `timeout`       | Number	  | Optional	  | [Amount of time](/docs/flowpipe-hcl/pipeline#timeout-argument) this step has to run before an error is raised. Defaults to `60s`.
| `title`         | String | Optional | A display title for the step.

-->