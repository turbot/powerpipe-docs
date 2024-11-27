---
title: Using Variables
---

# Using Variables
[Variables](/docs/powerpipe-hcl/variable) are module-level objects that allow you to pass values to your module at runtime.  When running Powerpipe, you can pass values on the command line or from a `.ppvars` file, and you will be prompted for any variables that have no values.

[Locals](/docs/powerpipe-hcl/locals) are internal, private variables used only *within* your mod - you cannot pass values in at runtime.

##  Input Variables

### Defining Input Variables
Powerpipe mods support input variables that are similar to [terraform input variables](https://www.terraform.io/docs/language/values/variables.html):

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
    - `tuple([<TYPE>, ...])`
  - The keyword `any` may be used to indicate that any type is acceptable 
- `description` - A description of the variable.  This text is included when the user is prompted for a variable's value.


### Using Input Variables
Variables may be referenced as `var.<NAME>`.  Variables are often used to pass [parameters](/docs/build/param-query) to queries:

```hcl
variable "instance_state" {
  type    = string
  default = "stopped" 
}

query "instances_in_state" {
  sql = "select instance_id, instance_state from aws_ec2_instance where instance_state = $1;" 
  param "find_state" {
    default = var.instance_state
  } 
}
```

### Passing Input Variables
When running Powerpipe, you can pass variables in several ways.  You can pass individual variable values on the command line with one or more `--var` arguments:

```bash
powerpipe query --var=instance_state="running"
```

When passing list variables, they must be enclosed in single quotes:

```bash
powerpipe benchmark run aws_tags.benchmark.mandatory --var='mandatory_tags=["Owner","Application","Environment"]' --var='prohibited_tags=["password","key"]'
```

You can specify variable values in a `.ppvars` file, using HCL syntax:
```hcl
mandatory_tags = [
  "Owner",
  "Application", 
  "Environment"
] 

prohibited_tags =[ 
  "password",
  "key"
]
```

Powerpipe *automatically* reads in the file named `powerpipe.ppvars` as well as any file ending in `.auto.ppvars` from the working directory if they exist.  You can also specify a variable file by name on the command line:
```bash
powerpipe cbenchmark run aws_tags.benchmark.mandatory --var-file='tags.ppvars'
```

You may also set variable values via environment variables.  Simply prefix the Powerpipe variable name with `PP_VAR_`:

```bash
export PP_VAR_mandatory_tags='["Owner","Application", "Environment"]' 
```

If you run Powerpipe from a mod that defines input variables, and they are not set anywhere (no default, not set in a `.ppvars` file, not set with `--var` argument, not set via an environment variable) then Powerpipe will prompt you for them before running the control/benchmark.

Powerpipe loads variables in the following order, with later sources taking precedence over earlier ones:
1. Environment variables
1. The `powerpipe.ppvars` file, if present.
1. Any `*.auto.ppvars` files, in alphabetical order by filename.
1. Any `--var` and `--var-file` options on the command line, in the order they are provided.


### Passing Variables for Dependency Mods

A Powerpipe mod can depend on other mods, and those dependency mods may include variables that you would like to pass.  To set them, prefix the variable names with the mod alias and then set them like any other variable.

You can set them in a `.ppvars` file:
```hcl
# direct dependency vars
aws_tags.mandatory_tags = ["Owner","Application","Environment"]
azure_tags.mandatory_tags = ["Owner","Application","Environment"]
```

Or pass them to the command with the `--var` argument
```bash
 powerpipe server --var 'aws_tags.mandatory_tags=["Owner","Application","Environment"]'  --var 'azure_tags.mandatory_tags=["Owner","Application","Environment"]' --var 'gcp_labels.mandatory_labels=["Owner","Application","Environment"]'
 ```

##  Local Variables
Powerpipe supports using local variables in a manner similar to [Terraform local values](https://www.terraform.io/docs/language/values/locals.html).  Unlike `variables`, locals cannot be passed in at runtime, but are useful as internal private variables.

The `locals` block defines and sets one or more local variables, using standard HCL assignment syntax.  The locals are scoped to the mod, and a mod may contain multiple `locals` blocks.  Locals may reference other values in the mod, including other local values.

A set of one or more local values can be declared in one or more `locals` blocks:
```hcl
locals {
  cis_v140_common_tags = {
    cis         = "true"
    cis_version = "v1.4.0"
    plugin      = "aws"
  }
}

locals {
  cis_v140_1_common_tags = merge(local.cis_v140_common_tags, {
    cis_section_id = "1"
  })
}
```

Once a local value is declared, you can reference it in expressions as `local.<NAME>`.
```hcl
control "cis_v140_1_1" {
  ...
  tags = local.cis_v140_1_common_tags
}
```
