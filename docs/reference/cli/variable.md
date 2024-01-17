---
title: powerpipe variable
sidebar_label: powerpipe variable
---

# powerpipe variable

Manage Powerpipe variables in the current mod and its direct dependents.

## Usage
```bash
powerpipe variable list [args]
powerpipe variable show variable_name [args]
```


## Sub-Commands

| Command | Description
|-|-
| [list](#powerpipe-variable-list) | List variables from the current mod and its direct dependents.
| [show](#powerpipe-variable-show) | Show details of a variable from the current mod or its direct dependents or from a Powerpipe server instance.


----
## powerpipe variable list
List variables from the current mod and its direct dependents.

### Examples


List variables:
```bash
powerpipe variable list
```


List all variables in `JSON` format:
```bash
powerpipe variable list --output json
```

List variables from a local server instance running on the default port on `localhost`:
```bash
powerpipe variable list --host local
```


List variables on a remote Powerpipe server instance:
```bash
powerpipe variable list --host  https://powerpipe.my-org.com:9194
```


List variables using settings from a workspace:
```bash
powerpipe variable list --workspace my_workspace
```


---

## powerpipe variable show
Show details of a variable from the current mod or its direct dependents or from a Powerpipe server instance.


### Examples

Show details of a single variable in the current mod:
```bash
powerpipe variable show mandatory_tags
```


Show details of a single variable in a direct dependency mod:
```bash
powerpipe variable show aws_tags.mandatory_tags
```

Show details of a variable on a Powerpipe server instance:
```bash
powerpipe variable show aws_tags.mandatory_tags --host https://powerpipe.my-org.com:9194
```


Show details of a variable in `JSON` format:
```bash
powerpipe variable show mandatory_tags --output json
```


Show details of a variable using settings from a workspace:
```bash
powerpipe variable show mandatory_tags -workspace my_workspace
```
---
