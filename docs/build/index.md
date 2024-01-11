---
title: Build Mods
sidebar_label: Build Mods
---

# Flowpipe Mods

Flowpipe allows you to write "pipelines as code", defining workflows and other tasks that are performed in a sequence.

Flowpipe resources include:
- **[Triggers](/docs/build/triggers)** - A way to initiate a pipeline (e.g. cron, webhook, etc).
- **[Pipelines](/docs/build/write-pipelines/index)** - A sequence of steps to do work.
- **[Variables, locals](/docs/build/mod-variables)** and standard HCL functions.


A Flowpipe **mod** is a portable, versioned collection of Flowpipe [pipelines](/docs/build/write-pipelines/index) and [triggers](/docs/build/triggers). Flowpipe mods and mod resources are defined in HCL, and distributed as simple text files.  Modules can be found on the [Flowpipe Hub](https://hub.flowpipe.io) and may be shared with others from any public git repository. 

Flowpipe mods are written in HCL. When Flowpipe runs, it will load the mod from the working directory and will read all files with the .fp extension from the directory and its subdirectories recursively.  

## Initializing a Mod

Creating a mod is simple.  First, create a directory for your mod.

```bash
mkdir my_mod
```

Change to your mod directory and run `flowpipe mod init` to initial your mod:
```bash
cd my_mod
flowpipe mod init
```

The `flowpipe mod init` command will create a file called `mod.fp` that contains a `mod` resource for mod named `local`:
```hcl
mod "local" {
  title = "my_mod"
}
```

You can use a text editor to modify the [mod definition](/docs/flowpipe-hcl/mod) in this file to give your mod a name, title, description, and other properties.

```hcl
mod "my_mod" {
  title = "My First Flowpipe Mod"
  description   = "Sample mod for Flowpipe documentation."
}
```

Your mod is initialized!  You can add [pipelines](/docs/build/write-pipelines/index) and [triggers](/docs/build/triggers) to your mod using [Flowpipe HCL](/docs/flowpipe-hcl/index).  You can even [use resources from other mods](/docs/build/mod-dependencies); explore the available mods on the [Flowpipe Hub](https://hub.flowpipe.io/)!
