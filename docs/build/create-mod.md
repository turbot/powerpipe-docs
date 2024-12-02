---
title: Creating a Mod
---

# Create a Mod

Creating a mod is simple. First, create a directory for your mod.
```bash
mkdir powerpipe-mod-demo
```


Change to your mod directory and run `powerpipe mod init` to initial your mod:

```bash
cd powerpipe-mod-demo/
powerpipe mod init
```

The `powerpipe mod init` command will create a file called `mod.pp` that contains a mod resource for mod named `local`:

```hcl
mod "local" {
  title = "powerpipe-mod-demo"
}
```

You can use a text editor to modify the mod definition in this file to give your mod a name, title, description, and other properties.

```hcl
mod "powerpipe_demo" {
  title = "Powerpipe Demo"
}

```

Your mod is initialized! You can add [dashboards](/docs/powerpipe-hcl/dashboard), [benchmarks](/docs/powerpipe-hcl/benchmark) and other resources to your mod using [Powerpipe HCL](/docs/powerpipe-hcl/). You can even [use resources from other mods](/docs/build/mod-dependencies). Explore the available mods on the [Powerpipe Hub](https://hub.powerpipe.io)!