---
title: Mod Dependencies
---

# Mod Dependencies

Powerpipe mods may depend on other mods, allowing you to quickly and easily extend them with additional features and functionality.  

To add a dependency, run `powerpipe mod install` from the root directory of your mod, specifying the path to the mod's Github repo:

```bash
cd my-mod
powerpipe mod install github.com/turbot/steampipe-mod-aws-compliance
```

This will install the mod into the `.powerpipe` sub-directory, and will add the dependency to the [require block](/docs/powerpipe-hcl/mod#require) of your `mod.pp` file:
```hcl
mod "local" {
  title = "my-mod"
  require {
    mod "github.com/turbot/steampipe-mod-aws-compliance" {
      version = "latest"
    }
  }
}
```


You can then create new `.pp` files in your mod that reference the resources in the dependency mods.  You can create your own controls that use `query` resources from the dependency mod: 

```hcl
control "my_mod_public_ec2" {
  title         = "EC2 instances should not have a public IP address"
  description   = "This control checks whether EC2 instances have a public IPv4 address."
  severity      = "high"
  sql           = aws_compliance.query.ec2_instance_not_publicly_accessible.sql
}
```

Or create your own dashboards or benchmarks that reference resources from your own mod or any dependencies:
```hcl
benchmark "my_mod_public_resources" {
  title       = "Public Resources"
  description = "Resources that are public."
  children = [
    aws_compliance.control.dms_replication_instance_not_publicly_accessible,
    aws_compliance.control.redshift_cluster_prohibit_public_access,
    aws_compliance.control.s3_bucket_restrict_public_read_access,
    aws_compliance.control.s3_bucket_restrict_public_write_access,
    control.my_mod_public_ec2,
  ]
}
```

You can add, remove, and update your dependencies with the [powerpipe mod command](/docs/reference/cli/mod). 

You can run the benchmarks in your mod:
```bash
powerpipe benchmark run aws_compliance.benchmark.nist_csf_pr_ac_3
```

When in a mod folder, you can run the dependent controls and benchmarks by qualifying them with the mod name:
```bash
powerpipe benchmark run aws_compliance.benchmark.cis_v140 
```

When running `powerpipe server` from a mod, all dashboards in your mod and its direct dependencies will be available to run.


## Installing Mods

### Git URLs & Private Repos

Powerpipe uses `git` to install and update mods. When you run `powerpipe mod install` or `powerpipe mod update`, Powerpipe will first try using HTTPS and if that does not work it will try SSH.  If your SSH keys are configured properly for `git`, you should be able to pull from private repos that you have access to, as well as public ones.  Alternatively, you can authenticate with a GitHub personal access token or application token.  Set the [POWERPIPE_GIT_TOKEN](/docs/reference/env-vars/powerpipe_git_token) environment variable to your token and Powerpipe will use the token when installing and updating mods.


### Mod Version Constraints

When installing a mod, you may specify a [semver constraint](https://semver.org/).  The latest version that meets the constraint will be installed, and the constraint will be added to the `mod.pp` and honored by subsequent `steampipe mod update` operations.

When installing the mod, append the mod repo with `@` and any valid semver constraint:

```bash
powerpipe mod install github.com/turbot/steampipe-mod-aws-insights@'^1'
powerpipe mod install github.com/turbot/steampipe-mod-aws-insights@1
powerpipe mod install github.com/turbot/steampipe-mod-aws-insights@1.x.x
powerpipe mod install github.com/turbot/steampipe-mod-aws-insights@'>=0.20'
```

### Installing from Branches and Tags

To install from a tagged commit, append the mod repo with `@` and the tag:
```bash
powerpipe mod install github.com/turbot/steampipe-mod-aws-insights@mycustomtag'
```
Note that the syntax is the same as for [semver constraints](#mod-version-constraints), and if the tag value is a valid semver string, Powerpipe will interpret it as a semver constraint and not a literal tag name.

To install from a branch, append the mod repo with `#` and the branch name:
```bash
powerpipe mod install github.com/turbot/steampipe-mod-aws-insights#main'
```

### Installing from the local filesystem
When developing mods, it can be useful to work from a local copy.  To install a mod from a local filesystem path, just pass the path to the install command:

```bash
powerpipe  mod install ../steampipe-mod-aws-insights
```


### Update Strategy

It is also possible to have more granular control of the update behavior - e.g. when to check for new commits. The `--pull` argument can be used to specify the update strategy when running `powerpipe update` or `powerpipe install`:

| Strategy | Description
|----------|---------------------------------------------------
| `full`   | Check branches and tags for both latest and accuracy
| `latest` | Update everything to latest, but only branches (not tags) are commit checked
| `development` | Update branches and broken constraints to latest, leave satisfied constraints unchanged
| `minimal`| Only update broken constraints. Do not check branches for new commits


### Publishing & Distributing mods
When publishing public mods, you should only depend on public mods (hosted in public repos) so that users of your mod don't encounter permissions issues - Avoid dependencies on local or private mods!

When users install your mod using `powerpipe mod install`, your dependencies will get installed automatically.  As a result, it is recommended that you add the `.powerpipe` directory to your `.gitignore` file and do not check these files into git.
