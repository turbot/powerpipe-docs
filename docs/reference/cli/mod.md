---
title: powerpipe mod
sidebar_label: powerpipe mod
---


# powerpipe mod

Powerpipe mod management.

Mods provide an easy way to share Powerpipe pipelines.  Find mods using the public registry at [hub.powerpipe.io](https://hub.powerpipe.io/).



## Usage
```bash
powerpipe mod init
powerpipe mod install [mod_path] [args]
powerpipe mod list [args]
powerpipe mod show mod_name [args]
powerpipe mod uninstall mod_name [args]
powerpipe mod update [mod_path] [args]
```

## Sub-Commands

| Command | Description
|-|-
| [init](#powerpipe-mod-init)       | Initialize the current directory with a `mod.pp` file 
| [install](#powerpipe-mod-install) | Install one or more mods and their dependencies
| [list](#powerpipe-mod-list)       | List mods from the current mod and its direct dependents.
| [show](#powerpipe-mod-show)       | Show details of a mod from the current mod or its direct dependents.
| [uninstall](#powerpipe-mod-uninstall) | Uninstall a mod and its dependencies
| [update](#powerpipe-mod-update)   | Update one or more mods and their dependencies



----
## powerpipe mod init
Initialize the current directory with a `mod.pp` file.

### Examples

Initialize a mod (create a `mod.pp`) in the current directory:

```bash
powerpipe mod init
```

Initialize a mod (create a `mod.pp`) in the another directory:

```bash
powerpipe mod init --mod-location ~/my_mod
```

---

## powerpipe mod install
Install one or more mods and their dependencies.

### Git URLs & Private Repos

Powerpipe uses `git` to install and update mods. When you run `powerpipe mod install` or `powerpipe mod update`, Powerpipe will first try using `https` and if that does not work it will try `ssh`.  If your SSH keys are configured properly for `git`, you should be able to pull from private repos that you have access to, as well as public ones.

When publishing mods, you should usually only depend on public mods (hosted in public repos) so that users of your mod don't encounter permissions issues.

### Arguments
| Flag | Description
|-|-
|` --dry-run` | Show which mods would be installed/updated/uninstalled without modifying them (default `false`).
|` --force` | Install mods even if plugin/cli version requirements are not met (cannot be used with `--dry-run`).
|` --prune` | Remove unused dependencies after installation is complete (default `true`).

### Examples

Install a mod and add the `require` statement to your `mod.pp`:
```bash
powerpipe mod install github.com/turbot/steampipe-mod-aws-insights
```

Install an exact version of a mod and update the `require` statement to your `mod.pp`.  This may upgrade or downgrade the mod if it is already installed:
```bash
powerpipe mod install github.com/turbot/steampipe-mod-aws-insights@0.1.0
```

Install a version of a mod using a semver constraint and update the `require` statement to your `mod.pp`.  This may upgrade or downgrade the mod if it is already installed:
```bash
powerpipe mod install github.com/turbot/steampipe-mod-aws-insights@'^1'
```

Install all mods specified in the `mod.pp` and their dependencies:
```bash
powerpipe mod install
```

Preview what `powerpipe mod install` will do, without actually installing anything:
```bash
powerpipe mod install --dry-run
```

---

## powerpipe mod list
List mods from the current mod and its direct dependents.

### Examples


List mods:
```bash
powerpipe mod list
```

List all mods in `JSON` format:
```bash
powerpipe mod list --output json
```

List mods using settings from a workspace:
```bash
powerpipe mod list --workspace my_workspace
```


---

## powerpipe mod show
Show details of a mod from the current mod or its direct dependents.


### Examples

Show details of a single mod in the current mod:
```bash
powerpipe mod show aws_compliance
```

Show details of a mod in `JSON` format:
```bash
powerpipe mod show aws_compliance --output json
```


Show details of a mod using settings from a workspace:
```bash
powerpipe mod show aws_compliance -workspace my_workspace
```

---

## powerpipe mod uninstall
Uninstall a mod and its dependencies.

### Arguments
| Flag | Description
|-|-
|` --dry-run` | Show which mods would be uninstalled without modifying them (default `false`).
|` --prune`   | Remove unused dependencies after uninstallation is complete (default `true`).

### Examples

Uninstall a mod:
```bash
powerpipe mod uninstall github.com/turbot/steampipe-mod-azure-insights
```

Preview uninstalling a mod, but don't uninstall it:
```bash
powerpipe mod uninstall github.com/turbot/steampipe-mod-gcp-insights --dry-run
```


----
## powerpipe mod update
Update one or more mods and their dependencies.

### Arguments

| Flag | Description
|-|-
|` --dry-run` | Show which mods would be updated without modifying them (default `false`).
|` --force` | Update mods even if plugin/cli version requirements are not met (cannot be used with `--dry-run`).
|` --prune` | Remove unused dependencies after update is complete (default `true`).



### Examples


Update a mod to the latest version allowed by its current constraint:
```bash
powerpipe mod update github.com/turbot/steampipe-mod-aws-insights
```

Update all mods specified in the `mod.pp` and their dependencies to the latest versions that meet their constraints, and install any that are missing:
```bash
powerpipe mod update
```

---
