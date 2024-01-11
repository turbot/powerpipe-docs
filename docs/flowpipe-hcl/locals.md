---
title: locals
sidebar_label: locals
---

# locals
The `locals` block defines and sets one or more [local variables](build/mod-variables#local-variables), using standard HCL assignment syntax.  The locals are scoped to the mod, and a mod may contain multiple `locals` blocks.  Locals may reference other values in the mod, including other local values.

You can reference local values as `local.<NAME>`.


## Example Usage

```hcl
locals {
  pipes_api_version = "latest"
  pipes_baseurl     = "https://pipes.turbot.com"
  pipes_cred_file   = "~/.steampipe/internal/pipes.turbot.com.tptt" 
}

locals {
  pipes_api_url     = "${local.pipes_baseurl}/api/${local.pipes_api_version}" 
}

pipeline "list_my_workspaces" {
  step "http" "list_workspaces" {
    url = "${local.pipes_api_url}/actor/workspace"
    request_headers = {
      Authorization = "Bearer ${file(local.pipes_cred_file)}"
    }
  }

  output "workspaces" {
    value = step.http.list_workspaces.response_body.items[*].handle
  }
}
```

