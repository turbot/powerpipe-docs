---
title: Mod Dependencies
sidebar_label: Mod Dependencies
---

# Mod Dependencies

Powerpipe mods may depend on other mods, allowing you to quickly and easily extend them with additional features and functionality.  

To add a dependency, run `powerpipe mod install` from the root directory of your mod, specifying the path to the mod's Github repo:

```bash
cd my-mod
powerpipe mod install github.com/turbot/powerpipe-mod-aws-compliance
```

This will install the mod into the `.powerpipe` sub-directory, and will add the dependency to the [require block](/docs/powerpipe-hcl/mod#require) of your `mod.sp` file:
```hcl
mod "local" {
  title = "my-mod"
  require {
    mod "github.com/turbot/powerpipe-mod-aws-compliance" {
      version = "latest"
    }
  }
}
```


You can then create new `.sp` files in your mod that reference the resources in the dependency mods.  You can create your own controls that use `query` resources from the dependency mod: 

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

You can run all the benchmarks in your mod:
```bash
powerpipe check all
```

When in a mod folder, `powerpipe check all` will only run benchmarks defined in the mod, however you can run the dependent controls and benchmarks by qualifying them with the mod name:
```
powerpipe check aws_compliance.benchmark.cis_v140 
```

Or you can run all benchmarks in a dependency mod by specifying only the mod name:
```bash
powerpipe check aws_compliance 
```


When running `powerpipe dashboard` from a mod, all dashboards in your mod and its direct dependencies will be available to run.

