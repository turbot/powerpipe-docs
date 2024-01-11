---
title: Using Variables
sidebar_label: Using Variables
---

# Using Variables

[Variables](flowpipe-hcl/variable) are module-level objects that allow you to pass values to your module at runtime.  When running Flowpipe, you can pass values on the command line or from a `.fpvars` file, and you will be prompted for any variables that have no values.

[Locals](flowpipe-hcl/locals) are internal, private variables used only *within* your mod - you cannot pass values in at runtime.

##  Input Variables

### Defining Input Variables
Flowpipe mods support input variables that are similar to [Terraform input variables](https://www.terraform.io/docs/language/values/variables.html):

You declare them with a `variable` block:
```hcl
variable "instance_id" {
  type = string
}

variable "mandatory_tag_keys" {
  type        = list(string)
  description = "A list of mandatory tag keys to check for (case sensitive)."
  default     = ["Environment", "Owner"]
}

```

You can optionally define:
- `default` - A default value.  If no value is passed, the user is not prompted and the default is used.
- `type` - The data type of the variable.  This may be a simple type or a collection.
  - The supported type primitives are:
    - `string`
    - `number`
    - `bool`
  - Collections types may also be used:
    - `list(<TYPE>)`
    - `set(<TYPE>)`
    - `map(<TYPE>)`
    - `object({<ATTR NAME> = <TYPE>, ... })`
  - The keyword `any` may be used to indicate that any type is acceptable 
- `description` - A description of the variable.  This text is included when the user is prompted for a variable's value.

### Using Input Variables
Variables may be referenced as `var.<NAME>`.  Variables are often used as defaults for pipeline parameters:

```hcl
pipeline "list_org_workspaces" {
  param "org" {
    type    = string
    default = var.pipes_org
  }

  step "http" "list_workspaces" {
    url    = "https://pipes.turbot.com/api/latest/org/${param.org}/workspace"
    method = "get"

    request_headers = {
      Authorization = "Bearer ${file("~/.steampipe/internal/pipes.turbot.com.tptt")}"
    }
  }

  output "workspaces" {
    value = step.http.list_workspaces.response_body.items[*].handle 
  }
}
```

### Passing Input Variables
When running Flowpipe, you can pass variables in several ways.  You can pass individual variable values on the command line with one or more `--var` arguments:

```bash
flowpipe pipeline run list_org_workspaces --var=org="my_org"
```

When passing list variables, they must be enclosed in single quotes:

```bash
flowpipe pipeline run check_tags --var='mandatory_tags=["Owner","Application","Environment"]' 
```

You can specify variable values in a `.fpvars` file, using HCL syntax:
```hcl
mandatory_tags = [
  "Owner",
  "Application", 
  "Environment"
] 

org = "my_org"
```

Flowpipe *automatically* reads in the file named `flowpipe.fpvars` as well as any file ending in `.auto.fpvars` from the working directory if they exist.  You can also specify a variable file by name on the command line:

```bash
flowpipe pipeline run check_tags  --var-file='tags.fpvars'
```

You may also set variable values via environment variables.  Simply prefix the Flowpipe variable name with `FP_VAR_`:

```bash
export FP_VAR_mandatory_tags='["Owner","Application", "Environment"]' 
```

If you run Flowpipe from a mod that defines input variables, and they are not set anywhere (no default, not set in a `.fpvars` file, not set with `--var` argument, not set via an environment variable) then Flowpipe will prompt you for them before running the pipeline.

Flowpipe loads variables in the following order, with later sources taking precedence over earlier ones:
1. Environment variables
1. The `flowpipe.fpvars` file, if present.
1. Any `*.auto.fpvars` files, in alphabetical order by filename.
1. Any `--var` and `--var-file` options on the command line, in the order they are provided.


### Passing Variables for Dependency Mods

A Flowpipe mod can depend on other mods, and those dependency mods may include variables that you would like to pass.  To set them, prefix the variable names with the mod name and then set them like any other variable.

You can set them in a `.fpvars` file:
```hcl
// direct dependency vars
aws_tags.mandatory_tags = ["Owner","Application","Environment"]
azure_tags.mandatory_tags = ["Owner","Application","Environment"]
```

Or pass them to the command with the `--var` argument
```bash
flowpipe pipeline run check_tags  --var 'aws_tags.mandatory_tags=["Owner","Application","Environment"]'  --var 'azure_tags.mandatory_tags=["Owner","Application","Environment"]' --var 'gcp_labels.mandatory_labels=["Owner","Application","Environment"]'
 ```

##  Local Variables
Flowpipe supports using local variables in a manner similar to [Terraform local values](https://www.terraform.io/docs/language/values/locals.html).  Unlike `variables`, locals cannot be passed in at runtime, but are useful as internal private variables.

The `locals` block defines and sets one or more local variables, using standard HCL assignment syntax.  The locals are scoped to the mod, and a mod may contain multiple `locals` blocks.  Locals may reference other values in the mod, including other local values.

A set of one or more local values can be declared in one or more `locals` blocks:
```hcl
locals {
  common_tags = {
    owner       = "dev team"
    environment = "prod"
  }
}
```

Once a local value is declared, you can reference it in expressions as `local.<NAME>`.
```hcl
pipeline "my_pipeline" {
  ...
  tags = local.common_tags
}
```
