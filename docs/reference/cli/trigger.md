---
title: flowpipe trigger
sidebar_label: flowpipe trigger
---

# flowpipe trigger

Manage Flowpipe variables in the current mod and its direct dependents.


## Usage
```bash
flowpipe trigger [command] [flags]
```

## Sub-Commands

| Command | Description
|-|-
| `list` | List triggers from the current mod.
| `show` | List a trigger from the current mod.


## Examples

List triggers:

```bash
flowpipe trigger list
```

View trigger details:

```bash
flowpipe trigger show my_trigger
```

List triggers in JSON format:

```bash
flowpipe trigger list --output json
```