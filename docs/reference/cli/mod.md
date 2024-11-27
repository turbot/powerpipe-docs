---
title: powerpipe mod
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

Initialize a mod (create a `mod.pp`) in another directory:

```bash
powerpipe mod init --mod-location ~/my_mod
```

---

## powerpipe mod install
Install one or more mods and their dependencies.  In addition to downloading the mod, Powerpipe will [add the mod dependency to the `mod.pp` file](/docs/powerpipe-hcl/mod#mod-1).

Powerpipe uses `git` to install and update mods. When you run `powerpipe mod install` or `powerpipe mod update`, Powerpipe will first try using HTTPS and if that does not work it will try SSH.  If your SSH keys are configured properly for `git`, you should be able to pull from private repos that you have access to, as well as public ones.  Alternatively, you can authenticate with a GitHUb personal access token or application token.  Set the [POWERPIPE_GIT_TOKEN](/docs/reference/env-vars/powerpipe_git_token) environment variable to your token and Powerpipe will use the token when installing and updating mods.

When you install a mod, the latest version is installed by default:
```bash
powerpipe mod install github.com/turbot/steampipe-mod-aws-insights
```

When installing a mod, you may specify a semver constraint.  The latest version that meets the constraint will be installed, and the constraint will be added to the `mod.pp` and honored by subsequent `steampipe mod update` operations.
```bash
powerpipe mod install github.com/turbot/steampipe-mod-aws-insights@1.x.x
```

To install from a tagged commit, append the mod repo with `@` and the tag:
```bash
powerpipe mod install github.com/turbot/steampipe-mod-aws-insights@mycustomtag
```
Note that the syntax is the same as for semver constraints, and if the tag value is a valid semver string, Powerpipe will interpret it as a semver constraint and not a literal tag name.

To install from a branch, append the mod repo with `#` and the branch name:
```bash
powerpipe mod install github.com/turbot/steampipe-mod-aws-insights#main
```

When developing mods, it can be useful to work from a local copy.  To install a mod from a local filesystem path, just pass the path to the install command:

```bash
powerpipe  mod install ../steampipe-mod-aws-insights
```

### Arguments
| Flag | Description
|-|-
| `--dry-run` | Show which mods would be installed/updated/uninstalled without modifying them (default `false`).
| `--force` | Install mods even if plugin/cli version requirements are not met (cannot be used with `--dry-run`).
| `--prune` | Remove unused dependencies after installation is complete (default `true`).
| `--pull string` | Specify an [update strategy](#update-strategy): `full`, `latest`, `development`, `minimal` (default is `latest` if there is a target, `minimal` if no target is given).


#### Update Strategy

It is also possible to have more granular control of the update behavior - e.g. when to check for new commits. The -`-pull` argument can be used to specify the update strategy when running `powerpipe update` or `powerpipe install`:

| Strategy | Description
|----------|---------------------------------------------------
| `full`   | Check branches and tags for both latest and accuracy
| `latest` | Update everything to latest, but only branches (not tags) are commit checked
| `development` | Update branches and broken constraints to latest, leave satisfied constraints unchanged
| `minimal`| Only update broken constraints. Do not check branches for new commits


### Examples

Install the latest version of a mod:
```bash
powerpipe mod install github.com/turbot/steampipe-mod-aws-insights
```

Install an exact version of a mod:
```bash
powerpipe mod install github.com/turbot/steampipe-mod-aws-insights@0.1.0
```

Install a version of a mod using a semver constraint:
```bash
powerpipe mod install github.com/turbot/steampipe-mod-aws-insights@'^1'
powerpipe mod install github.com/turbot/steampipe-mod-aws-insights@1
powerpipe mod install github.com/turbot/steampipe-mod-aws-insights@1.x.x
powerpipe mod install github.com/turbot/steampipe-mod-aws-insights@'>=0.20'
```

Install a mod from a GitHub tag:
```bash
powerpipe mod install github.com/turbot/steampipe-mod-aws-insights@my-tag
```

Install a mod from a GitHub branch:
```bash
powerpipe mod install github.com/turbot/steampipe-mod-aws-insights#main
```

Install a mod from a local directory:
```bash
powerpipe  mod install ../steampipe-mod-aws-insights
```

Install all mods specified in the `mod.pp` and their dependencies:
```bash
powerpipe mod install
```

Preview what `powerpipe mod install` will do, without actually installing anything:
```bash
powerpipe mod install --dry-run
```

Install all missing mods specified in the `mod.pp` and update all their dependencies:
```bash
powerpipe mod install --pull full
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
| `--pull string` | Specify an [update strategy](#update-strategy): `full`, `latest`, `development`, `minimal` (default `latest`)


### Examples


Update a mod to the latest version allowed by its current constraint:
```bash
powerpipe mod update github.com/turbot/steampipe-mod-aws-insights
```

Update all mods specified in the `mod.pp` and their dependencies to the latest versions that meet their constraints, and install any that are missing:
```bash
powerpipe mod update
```


Update all mods specified in the `mod.pp` and their dependencies to the latest versions that meet their constraints, and install any that are missing.  Check branches and tags for both latest and accuracy:
```bash
powerpipe mod update --pull full
```

---
