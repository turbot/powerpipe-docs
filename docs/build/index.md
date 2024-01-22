---
title: Build Mods
sidebar_label: Build Mods
---
# Powerpipe Mods


A Powerpipe **mod** is a portable, versioned collection of related Powerpipe resources such as dashboards, benchmarks, queries, and controls. Powerpipe mods and mod resources are defined in HCL, and distributed as simple text files.  Modules can be found on the [Powerpipe Hub](https://hub.powerpipe.io), and may be shared with others from any public git repository. 

Mods provide an easy way to share dashboards, benchmarks, and other resources.

You can download and install mods from the [Powerpipe Hub](https://hub.powerpipe.io) and use them as-is; [view the dashboards](/docs/run/dashboard), [run benchmarks](/docs/run/benchmark), and [take snapshots](/docs/run/snapshots/) without writing code.

Powerpipe also makes it simple to create dashboards and benchmarks and to share them in your own mod!

## Creating a Mod


Creating a mod is simple. First, create a directory for your mod.
```bash
mkdir my_mod
```


Change to your mod directory and run `powerpipe mod init` to initial your mod:

```bash
cd my_mod
powerpipe mod init
```

The `powerpipe mod init` command will create a file called `mod.pp` that contains a mod resource for mod named `local`:

```hcl
mod "local" {
  title = "my_mod"
}
```

You can use a text editor to modify the mod definition in this file to give your mod a name, title, description, and other properties.

```hcl
mod "my_mod" {
  title = "My First Powerpipe Mod"
  description   = "Sample mod for Powerpipe documentation."
}
```

Your mod is initialized! You can add [dashboards](/docs/powerpipe-hcl/dashboard), [benchmarks](/docs/powerpipe-hcl/benchmark) and other resources to your mod using [Powerpipe HCL](/docs/powerpipe-hcl/). You can even [use resources from other mods](/docs/build/mod-dependencies); explore the available mods on the [Powerpipe Hub](https://hub.powerpipe.io)!