---
title: powerpipe variable
sidebar_label: powerpipe variable
---

# powerpipe variable

Manage powerpipe variables in the current mod and its direct dependents.


## Usage
```bash
powerpipe variable [command] [flags]
```

## Sub-Commands

| Command | Description
|-|-
| `list` | List variables for the the current mod and its direct dependents.

## Flags

| Flag | Applies to | Description
|-|-|-
| `--mod-location string` | `list` | 
| `--output string` | `list` |  Select a console output format: `table` or `json` (default `table`)

## Examples

List variables:

```bash
powerpipe variable list
```


List variables in json format:

```bash
powerpipe variable list --output json
```