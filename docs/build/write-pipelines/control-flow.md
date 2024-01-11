---
title: Control Flow
sidebar_label: Control Flow
---

# Control Flow

By default, all steps are run in parallel:

```hcl
pipeline "my_parallel_pipeline" {
    step "http" "my_step_1" {
        url = "https://example.com/my/webhook1"
    }

    step "http" "my_step_2" {
        url = "https://example.com/my/webhook1"
    }
}
```

Flowpipe will define implicit dependencies based on references to other steps and run them in the required order:

```hcl
pipeline "my_serial_pipeline" {
    step "http" "my_step_1" {
        url = "https://example.com/my/webhook1"
    }

    step "http" "my_step_2" {
        url   = "https://example.com/my/webhook2"
        body  = step.http.my_step_1.response_body
    }
}
```

You can also define explicit dependencies with `depends_on` to make them run in a specific order

```hcl
pipeline "my_serial_pipeline" {
    step "http" "my_step_1" {
        url = "https://example.com/my/webhook1"
    }

    step "http" "my_step_2" {
        url         = "https://example.com/my/webhook2"
        depends_on  = [step.http.my_step_1]
    }
}
```
