---
title: flowpipe mod
sidebar_label: flowpipe mod
---

# flowpipe mod
Flowpipe mod management.

Mods provide an easy way to share Flowpipe pipelines.  Find mods using the public registry at [hub.flowpipe.io](https://hub.flowpipe.io/).


## Usage
```bash
flowpipe mod [command]
```

## Available Commands:

| Command | Description
|-|-
| `init`        | Initialize the current directory with a `mod.fp` file 
| `install`     | Install one or more mods and their dependencies
| `list`        | List currently installed mods
| `uninstall`   | Uninstall a mod and its dependencies
| `update`      | Update one or more mods and their dependencies


| Flag | Description
|-|-
|` --dry-run` | Show which mods would be installed/updated/uninstalled without modifying them (default `false`).
|` --prune` | Remove unused mods and dependencies when doing `mod update` and `mod install` (default `true`).


## Git URLs & Private Repos

Flowpipe uses `git` to install and update mods. When you run `flowpipe mod install` or `flowpipe mod update`, Flowpipe will first try using `https` and if that does not work it will try `ssh`.  If your ssh keys are configured properly for `git`, you should be able to pull from private repos that you have access to, as well as public ones.

When publishing mods, you should usually only depend on public mods (hosted in public repos) so that users of your mod don't encounter permissions issues.


## Examples
List installed mods:
```bash
flowpipe mod list
```

Install a mod and add the `require` statement to your `mod.fp`:
```bash
flowpipe mod install github.com/turbot/flowpipe-mod-aws
```

Install an exact version of a mod and update the `require` statement to your `mod.fp`.  This may upgrade or downgrade the mod if it is already installed:
```bash
flowpipe mod install github.com/turbot/flowpipe-mod-aws@0.1.0
```

Install a version of a mod using a semver constraint and update the `require` statement to your `mod.fp`.  This may upgrade or downgrade the mod if it is already installed:
```bash
flowpipe mod install github.com/turbot/flowpipe-mod-aws@'^1'
```

Install all mods specified in the `mod.fp` and their dependencies:
```bash
flowpipe mod install
```

Preview what `flowpipe mod install` will do, without actually installing anything:
```bash
flowpipe mod install --dry-run
```


Update a mod to the latest version allowed by its current constraint:
```bash
flowpipe mod update github.com/turbot/flowpipe-mod-aws
```

Update all mods specified in the `mod.fp` and their dependencies to the latest versions that meet their constraints, and install any that are missing:
```bash
flowpipe mod update
```


Uninstall a mod:
```bash
flowpipe mod uninstall github.com/turbot/flowpipe-mod-azure
```

Preview uninstalling a mod, but don't uninstall it:
```bash
flowpipe mod uninstall github.com/turbot/flowpipe-mod-gcp --dry-run
```
