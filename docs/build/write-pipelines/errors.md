---
title: Error Handling
sidebar_label: Error Handling
---

# Error Handling

Not everything goes according to plan, and Flowpipe provides error handling capabilities to help you handle unexpected failures.  


#### The `errors` Attribute
Each step has an `errors` attribute that contains a list of errors that occurred:

```hcl
"errors": [
    {
        "pipeline_execution_id": "pexec_clsskbpesk9lj8o9jcfg",
        "step_execution_id": "sexec_clsskbpesk9lj8o9jchg",
        "pipeline": "test_suite_mod.pipeline.error_in_for_each",
        "step": "http.bad_http",
        "error": {
            "instance": "fperr_clsskbpesk9lj8o9jcig",
            "type": "error_not_found",
            "title": "Not Found",
            "status": 404,
            "detail": "404 Not Found"
        }
    },
    {
        "pipeline_execution_id": "pexec_clsskbpesk9lj8o9jcfg",
        "step_execution_id": "sexec_clsskbpesk9lj8o9jcgg",
        "pipeline": "test_suite_mod.pipeline.error_in_for_each",
        "step": "http.bad_http",
        "error": {
            "instance": "fperr_clsskbpesk9lj8o9jcj0",
            "type": "error_not_found",
            "title": "Not Found",
            "status": 404,
            "detail": "404 Not Found"
        }
    },
    {
        "pipeline_execution_id": "pexec_clsskbpesk9lj8o9jcfg",
        "step_execution_id": "sexec_clsskbpesk9lj8o9jch0",
        "pipeline": "test_suite_mod.pipeline.error_in_for_each",
        "step": "http.bad_http",
        "error": {
            "instance": "fperr_clsskbpesk9lj8o9jci0",
            "type": "error_not_found",
            "title": "Not Found",
            "status": 404,
            "detail": "404 Not Found"
        }
    }
]
```

For steps with a `for_each`, each step instance will have an `errors` attribute containing the errors for that instance (e.g. `step.http.my_request["value"].errors`), but the step itself will also have an `errors` attribute (e.g. `step.http.my_request.errors`) containing a flattened union of all errors from all instances of the step.  Likewise, the pipeline will have an `errors` attribute that contains a flattened union of all errors from all instances of all steps in the pipeline.  Note that because errors are flattened and passed up through the stack, the error items must have enough execution context to look up full details from the log.

To simplify common error handling cases, Flowpipe provides some helper functions:
- `is_error`:  Given a reference to a step, `is_error` returns a boolean `true` if there are 1 or more errors, or false it there are no errors.
  - example:  `is_error(step.http.my_request)` 
  - This is equivalent to `length(step.http.my_request.errors) > 0`
-  `error_message`:  Given a reference to a step, `error_message` will return a string containing the first error message, if any. If there are no errors, then it will return an empty string.  This is useful for simple step primitives.
  - example: `error_message(step.http.my_request)`    


#### Handling Errors
What condition constitutes an error is dependent on the type of action being performed.  As a result, each step type (`http`, `query`, etc) will decide when to raise an error.

By default, all errors are fatal and are not retried — when a step encounters an error, it causes the step the fail.  A failed step results in a failed pipeline. Any step instances that are already running will complete (but will not be retried), and the pipeline will stop with a failed status.

##### error
You can override the default error behavior with an `error` block on the step. 

You can ignore the error and continue with `ignore = true`.  

```hcl
pipeline "subscribe" {
  param "subscriber" {
    type = "string"
  }

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
}
```

You can then "handle" the error in subsequent steps if you want:

```hcl
pipeline "subscribe" {
  param "subscriber" {
    type = "string"
  }

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

  step "email" "send_error" {
    to      = "admin@my_company.local"
    subject = "subscription error"
    body    = "Error - could not subscribe"
    if      = is_error(step.http.my_request)
  }
}
```


You may include an `if` argument in the `error` block to only ignore if a condition is met.  You can use the special value `result` to evaluate the attributes of the completed step instance (you can use `result` in `throw`, `retry`, and `loop` blocks as well).  `result` is essentially a self-reference to "this" step after it has run (e.g. the attributes are populated).  


```hcl
pipeline "subscribe" {
  param "subscriber" {
    type = "string"
  }

  step "http" "my_request" {
    url           = "https://myapi.local/subscribe"
    method        = "post"
    body          = jsonencode({
      name = param.subscriber
    })

    error {
      if    = result.status_code == 403
      ignore = true
    }
  }
}
```

A step may have no more than 1 `error` block.


##### retry

Alternatively, you can retry the step when it fails with a `retry` block:


```hcl
pipeline "subscribe" {
  param "subscriber" {
    type = "string"
  }

  step "http" "my_request" {
    url           = "https://myapi.local/subscribe"
    method        = "post"
    body          = jsonencode({
      name = param.subscriber
    })

    retry {
      max_attempts = 3
    }
  }
}
```

In fact, you can retry and *then* ignore the error even if all retries fail (`retry` is handled before `error`):


```hcl
pipeline "subscribe" {
  param "subscriber" {
    type = "string"
  }

  step "http" "my_request" {
    url           = "https://myapi.local/subscribe"
    method        = "post"
    body          = jsonencode({
      name = param.subscriber
    })

    retry {
      max_attempts = 3
    }

    error {
      ignore  = true
    }
  }
}
```

You may want to retry in some situations but not in others.  You can use `if` to conditionally handle the error.  Flowpipe will make the `result` object available inside the `retry` block.  This object is a "magic" reference to the resource that threw the error - in this case the `step` with the current state after this attempt. You can inspect the `result` to get any argument or attribute for the step in its current state:

```hcl
pipeline "subscribe" {
  param "subscriber" {
    type = "string"
  }

  step "http" "my_request" {
    url           = "https://myapi.local/subscribe"
    method        = "post"
    body          = jsonencode({
      name = param.subscriber
    })

    retry {
      if           = result.status_code == 429
      max_attempts = 3
    }
  }
}
```

You may specify additional arguments to control the retry behavior:

| Arg          | Default    | Description
|--------------|------------|-------------
| max_attempts | 3          | Specifies the maximum number of attempts to run the step.
| strategy     | `constant` | The backoff strategy.  One of `exponential`, `linear`, `constant`.
| min_interval | 1000       | The first interval between retries, in milliseconds.  If the strategy is `exponential` or `linear`, subsequent intervals will be scaled based on this value.
| max_interval | 10000      | The maximum interval between retries, in milliseconds.

You can do exponential backoff:
```
retry {
  max_attempts = 8
  strategy     = "exponential"
  min_interval = 100
  max_interval = 10000
}
```
or linear backoff:
```
retry {
  max_attempts = 3
  strategy     = "linear"
  min_interval = 1000
  max_interval = 10000
}
```

or constant intervals:
```
retry {
  max_attempts = 5
  strategy     = "constant"
  min_interval = 1000
}
```

The algorithm for the strategy is roughly:

```golang
def calculate_delay(attempt, strategy, interval, max_interval):
    if attempt == 1:
        return 0  # Immediate first attempt

    if strategy == "constant":
        return min_interval

    elif strategy == "linear":
        delay = (attempt - 1) * min_interval

    elif strategy == "exponential":
        delay = min_interval * (2 ** (attempt - 2))

    # Ensuring delay does not exceed max_interval
    return min(delay, max_interval)
```

A step may have no more than 1 `retry` block.


##### throw

You can also explicitly raise an exception with `throw`.  You may throw to raise an error when the step would usually succeed. You can include as many `throw` blocks as you want. Thrown errors are not retried, though they can be ignored.


```hcl
pipeline "subscribe" {
  param "subscriber" {
    type = "string"
  }

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
}
```


Note that if you `throw` an error, you may choose to ignore it with an `error` block, but you cannot `retry` it.  If the step has a `loop`, it will be evaluated last, only if there are no unhandled errors.
