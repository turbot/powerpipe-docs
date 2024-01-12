---
title: powerpipe mod
sidebar_label: powerpipe mod
---

# powerpipe mod
Powerpipe mod management.

Mods provide an easy way to share Powerpipe pipelines.  Find mods using the public registry at [hub.powerpipe.io](https://hub.powerpipe.io/).


## Usage
```bash
powerpipe mod [command]
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

Powerpipe uses `git` to install and update mods. When you run `powerpipe mod install` or `powerpipe mod update`, Powerpipe will first try using `https` and if that does not work it will try `ssh`.  If your ssh keys are configured properly for `git`, you should be able to pull from private repos that you have access to, as well as public ones.

When publishing mods, you should usually only depend on public mods (hosted in public repos) so that users of your mod don't encounter permissions issues.


## Examples
List installed mods:
```bash
powerpipe mod list
```

Install a mod and add the `require` statement to your `mod.fp`:
```bash
powerpipe mod install github.com/turbot/powerpipe-mod-aws
```

Install an exact version of a mod and update the `require` statement to your `mod.fp`.  This may upgrade or downgrade the mod if it is already installed:
```bash
powerpipe mod install github.com/turbot/powerpipe-mod-aws@0.1.0
```

Install a version of a mod using a semver constraint and update the `require` statement to your `mod.fp`.  This may upgrade or downgrade the mod if it is already installed:
```bash
powerpipe mod install github.com/turbot/powerpipe-mod-aws@'^1'
```

Install all mods specified in the `mod.fp` and their dependencies:
```bash
powerpipe mod install
```

Preview what `powerpipe mod install` will do, without actually installing anything:
```bash
powerpipe mod install --dry-run
```


Update a mod to the latest version allowed by its current constraint:
```bash
powerpipe mod update github.com/turbot/powerpipe-mod-aws
```

Update all mods specified in the `mod.fp` and their dependencies to the latest versions that meet their constraints, and install any that are missing:
```bash
powerpipe mod update
```


Uninstall a mod:
```bash
powerpipe mod uninstall github.com/turbot/powerpipe-mod-azure
```

Preview uninstalling a mod, but don't uninstall it:
```bash
powerpipe mod uninstall github.com/turbot/powerpipe-mod-gcp --dry-run
```
