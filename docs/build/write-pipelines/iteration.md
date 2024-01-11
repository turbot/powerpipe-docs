---
title: Iteration
sidebar_label: Iteration
---

# Iteration

## for_each
You can run a step in a parallel loop using `for_each`:


```hcl
pipeline "send_keys_to_slack" {
    param "access_keys" {
        type = "map"

        default = {
          AKIAXYSEVFAKE0123456 = {
            user_name = "jenny"
            account_id = "000008675309"
          }
          AKIAXYSEVFAKE0123456 = {
            user_name = "janderson"
            account_id = "000000090125"
          }
        }
    }

    step "http" "send_to_slack" {
        for_each = param.access_keys
        url      = var.slack_webhook_url
        method   = "post"

        request_body = jsonencode({
            text = "New AWS IAM access key: ${each.key} for user ${each.value.user_name} in ${each.value.account_id}"
        })
    }
}
```
<!--
By default, Flowpipe will run as many step instances in parallel as it can, but you can specify `max_parallel` to limit the number of concurrent step instances. For example, to run the steps in the loop sequentially, one at a time, set `max_parallel = 1`:


```hcl
pipeline "send_keys_to_slack" {
    param "access_keys" {
        type = "map"

        default = {
          AKIAXYSEVFAKE0123456 = {
            user_name = "jenny"
            account_id = "000008675309"
          }
          AKIAXYSEVFAKE0123456 = {
            user_name = "janderson"
            account_id = "000000090125"
          }
        }
    }

    step "http" "send_to_slack" {
        for_each     = param.access_keys
        url          = var.slack_webhook_url
        method       = "post"
        max_parallel = 1

        request_body = jsonencode({
            text = "New AWS IAM access key: ${each.key} for user ${each.value.user_name} in ${each.value.account_id}"
        })
    }
}
```
-->

Note that `for_each` accepts a *map* or a *list* of strings:

```hcl
pipeline "add_users" {
    param "users" {
        type    = "list"
        default = ["jerry","Janis", "Jimi"]
    }

    step "http" "add_a_user" {
        for_each = param.users
        url      = "https://myapi.local/api/v1/user"
        method   = "post"

        request_body = jsonencode({
            user_name = "${each.value}" 
        })
    }
}
```

When using the list form, `each.key` will be the index of the current item in the list, and `each.value` is the actual list item.

You may need to reference an "iterated" step in an `output` or another `step`.  These step instances are identified by a map key (or set member) from the value provided to `for_each`:
- `step.<TYPE>.<NAME>`  (e.g.`step.http.add_a_user`) refers to the block.  Note that `depends_on` will use the block reference, and that the dependency means *all* instances of the step.  You cannot depend on a specific instance of a step from a `for_each` loop.
- `step.<TYPE>.<NAME>[<KEY>]` (e.g.`step.http.add_a_user["0"]`, `step.http.send_to_slack["AKIAXYSEVFAKE0123456"]`) refers to an individual step instance.

For example, to wait until all the `add_a_user` steps have completed before sending a notification:
```hcl
pipeline "add_users" {
    param "users" {
        type    = "list"
        default = ["jerry","Janis", "Jimi"]
    }

    step "http" "add_a_user" {
        for_each = param.users
        url      = "https://myapi.local/api/v1/user"
        method   = "post"

        request_body = jsonencode({
            user_name = "${each.value}" 
        })
    }
    
    step "http" "notify_slack" {
        depends_on = [step.http.add_a_user]
        url        = var.slack_webhook_url

        request_body = jsonencode({
            text = "New users created: ${join(",", param.users)}"
        })
    }
}
```

 The step instances for a `for_each` are represented by a map, you can chain steps together where there is a 1:1 relationship between them (see [Terraform resource chaining](https://developer.hashicorp.com/terraform/language/meta-arguments/for_each#chaining-for_each-between-resources)).  For instance, we can modify the previous example to send a separate notification for each user:

```hcl
pipeline "add_users" {
    param "users" {
        type    = "list"
        default = ["jerry","Janis", "Jimi"]
    }

    step "http" "add_a_user" {
        for_each = param.users
        url      = "https://myapi.local/api/v1/user"
        method   = "post"

        request_body = jsonencode({
            user_name = "${each.value}" 
        })
    }

    
    step "http" "notify_slack" {
        for_each = step.http.add_a_user
        url      = var.slack_webhook_url

        request_body = jsonencode({
            text = "New user created: ${jsondecode(each.value.request_body)["user_name"]}"
        })
    }
}
```

In fact, you can  use `for_each` and `if` in the same step (`for_each` is evaluated before `if`), so you could improve the example:


```hcl
pipeline "add_users" {
    param "users" {
        type    = "list"
        default = ["jerry","Janis", "Jimi"]
    }

    step "http" "add_a_user" {
        for_each = param.users
        url      = "https://myapi.local/api/v1/user"
        method   = "post"

        request_body = jsonencode({
            user_name = "${each.value}" 
        })
    }

    step "http" "notify_success" {
        for_each = step.http.add_a_user
        if       = each.value.status_code >= 200 && each.value.status_code < 300
        url      = var.slack_webhook_url

        request_body = jsonencode({
            text = "New user created: ${jsondecode(each.value.request_body)["user_name"]}"
        })
    }

    step "http" "notify_failed" {
        for_each = step.http.add_a_user
        if       = each.value.status_code < 200 || each.value.status_code >= 300
        url      = var.slack_webhook_url

        request_body = jsonencode({
            text = "ERROR: New user creation failed: ${jsondecode(each.value.request_body)["user_name"]}"
        })
    }
}
```


## loop
You can also run a step in a sequential loop, changing the arguments with each iteration.  This is useful for handling HTTP pagination, for example.  To iterate the step, include a `loop` block.  The `until` condition is used to break the loop - Flowpipe will run the step again unless the `until` condition is `true`.  Once the `until` condition is true the loop is broken, the step completes, and execution continues at the next step. 

The loop is evaluated ***last*** after the step instance has been executed and all retries have been completed.  You can use the special value `result` to evaluate the attributes of the completed step instance (you can use `result` in an error handling block like `throw`, `error`, and `retry` as well).  `result` is essentially a self-reference to "this" step after it has run (e.g. the attributes are populated).  

When the step runs again due to the loop, it inherits all of the arguments from the step, but you can override them inside the `loop` block if desired, allowing you to pass data from the step execution into the next step execution.  For instance, you may extract a pagination token from an HTTP response, and then override the step's `url` argument and pass that token in as a querystring parameter:


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
      url   = "https://latestpipe.turbot.io/api/v1/org/latesttank/workspace/?limit=3&next_token=${result.response_body.next_token}"
    }

  }

```

There is a special attribute called `loop`  with a single attribute `index` which is the (zero-based) index of the loop count:

```hcl
pipeline "simple_loop" {

    step "echo" "repeat" {
        text  = "iteration ${loop.index}"


        loop {
            if = loop.index < 3
        }
    }
}
```