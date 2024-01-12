---
title: powerpipe mod
sidebar_label: powerpipe mod
---


# powerpipe mod
Powerpipe mod management.

Mods provide an easy way to share Powerpipe queries, controls, and benchmarks.  Find mods using the public registry at [hub.powerpipe.io](https://hub.powerpipe.io/mods).


## Usage
```bash
powerpipe mod [command]
```

## Available Commands:

| Command | Description
|-|-
| `init`        | Initialize the current directory with a `mod.sp` file 
| `install`     | Install one or more mods and their dependencies
| `list`        | List currently installed mods
| `uninstall`   | Uninstall a mod and its dependencies
| `update `     | Update one or more mods and their dependencies


| Flag | Description
|-|-
|` --dry-run` | Show which mods would be installed/updated/uninstalled without modifying them (default `false`).
|` --mod-location` | Sets the Powerpipe workspace working directory. If not specified, the workspace directory will be set to the current working directory. See <a href="reference/env-vars/powerpipe_mod_location">STEAMPIPE_MOD_LOCATION</a> for details.
|` --prune` | Remove unused mods and dependencies when doing `mod update` and `mod install` (default `true`).



## Examples
List installed mods:
```bash
powerpipe mod list
```

Install a mod and add the `require` statement to your `mod.sp`:
```bash
powerpipe mod install github.com/turbot/powerpipe-mod-aws-compliance
```

Install an exact version of a mod and update the `require` statement to your `mod.sp`.  This may upgrade or downgrade the mod if it is already installed:
```bash
powerpipe mod install github.com/turbot/powerpipe-mod-aws-compliance@0.1
```

Install a version of a mod using a semver constraint and update the `require` statement to your `mod.sp`.  This may upgrade or downgrade the mod if it is already installed:
```bash
powerpipe mod install github.com/turbot/powerpipe-mod-aws-compliance@'^1'
```

Install all mods specified in the `mod.sp` and their dependencies:
```bash
powerpipe mod install
```

Preview what `powerpipe mod install` will do, without actually installing anything:
```bash
powerpipe mod install --dry-run
```


Update a mod to the latest version allowed by its current constraint:
```bash
powerpipe mod update github.com/turbot/powerpipe-mod-aws-compliance
```

Update all mods specified in the `mod.sp` and their dependencies to the latest versions that meet their constraints, and install any that are missing:
```bash
powerpipe mod update
```


Uninstall a mod:
```bash
powerpipe mod uninstall github.com/turbot/powerpipe-mod-azure-compliance
```

Preview uninstalling a mod, but don't uninstall it:
```bash
powerpipe mod uninstall  github.com/turbot/powerpipe-mod-gcp-compliance --dry-run
```
