---
title: mod
sidebar_label: mod
---


# mod
Every mod must contain a `mod.fp` file with a single `mod` block.

The `mod` block contains metadata for the mod (including metadata used in the hub site and social media), as well as [dependency data](#mod-dependencies).  A mod author may edit the mod block directly, but Flowpipe will **also** edit the file, adding, removing, and modifying dependencies in the file when users add and remove mods via the [`flowpipe mod` commands](/docs/reference/cli/mod).  For this reason, it is recommended that the `mod.fp` *only* contain a `mod` block; do not add other mod resources (`trigger`, `pipeline`, etc) to this file.

The block label is the mod name.  Mod names use lower_snake_case. They may contain lowercase chars, numbers, or underscores, and must start with a letter.


## Example - Library mod with Hub Metadata
```hcl
mod "aws" {
  # hub metadata
  title         = "AWS Library"
  description   = "Run pipelines to supercharge your AWS workflows using Flowpipe."
  color         = "#FF9900"
  documentation = file("./docs/index.md")
  icon          = "/images/flowpipe/build/turbot/aws.svg"
  categories    = ["aws", "library"]

  opengraph {
    title       = "AWS Library Mod for Flowpipe"
    description = "Run pipelines to supercharge your AWS workflows using Flowpipe."
    image       = "/images/flowpipe/build/turbot/aws-social-graphic.png"
  }

  require {
    flowpipe {
      min_version = "0.1.0"
    }
  }
}

```

## Example - Composite Mod with Dependency

```hcl
mod "deactivate_expired_aws_iam_access_keys" {
  title       = "Deactivate expired AWS IAM keys"
  description = "Deactivates AWS IAM keys that have been active for a certain period of time."

  require {
    flowpipe {
      min_version = "0.1.0"
    }

    mod "github.com/turbot/flowpipe-mod-aws" {
      version = "v0.0.2-dev-samples.3"
      args = {
        region                = var.aws_region
        access_key_id         = var.aws_access_key_id
        secret_access_key     = var.aws_secret_access_key
      }
    }
  }

}
```

## Argument Reference

| Name | Type | Required? | Description
|-|-|-|-
| `categories` | List(String) | Optional | A list of labels, used to categorize mods (such as on the Flowpipe Hub).
| `color` | String |Optional |  A hexadecimal RGB value to use as the color scheme for the mod on hub.flowpipe.io.  
| `description` |  String | Optional | A string containing a short description. 
| `documentation` | String (Markdown)| Optional | A markdown string containing a long form description, used as documentation for the mod on hub.flowpipe.io. 
| `icon` |  String | Optional | The URL of an icon to use for the mod on hub.flowpipe.io.
| `opengraph` |  Block | Optional | Block of metadata for use in social media applications that support [Opengraph](#opengraph) metadata.
| `require` | Block | Optional | A block that specifies one or more [mod dependencies](#mod-dependencies).
| `tags` | Map | Optional | A map of key:value metadata for the mod, used to categorize, search, and filter.   
| `title` | String | Optional | The display title of the mod.


#### opengraph
The `opengraph` block is an optional block of metadata for use in social media applications that support [Opengraph](https://ogp.me/) metadata.

| Name | Type| Description
|-|-|-
| `description` | String | The Opengraph description (`og:description`) of the mod, for use in social media applications.
| `title` | String | The Opengraph display title (`og:title`) of the mod, for use in social media applications.

 

#### Mod Dependencies
A mod may contain a `require` block to specify version dependencies for the Flowpipe CLI and other mods.  While it is possible to edit this section manually, Flowpipe will also modify it (including reordering and removing comments) when you run a `flowpipe mod` command to install, update, or uninstall a mod.

A mod may specify a dependency on the Flowpipe CLI.  Flowpipe will evaluate the dependency when the mod is loaded and will error if the constraint is not met, but it will not install or upgrade the CLI.  A `flowpipe` constraint specifies a *minimum version*, and does not support semver syntax:
```hcl
flowpipe {
  min_version = "0.1.0"
}
```

<!--
A mod may specify a dependency on one or more plugins.  Flowpipe will evaluate the dependency when the mod is loaded and will error if the constraint is not met, but it will not install or upgrade the plugin. A `plugin` constraint specifies a *minimum version*, and does not support semver syntax:
```hcl
require {
  plugin "aws"{
    version = "0.24"
  }
}
```
-->

A mod may specify dependencies on other mods.  While you can manually edit the `mod` dependencies in the `mod.fp`, they are more commonly managed by Flowpipe when you install, update, or uninstall mods via the [flowpipe mod commands](/docs/reference/cli/mod).  The `version` can be an exact version <!-- ,a tag name, a branch name, a local file --> or a [semver](https://semver.org/) string:

```hcl
require {
  mod "github.com/turbot/flowpipe-mod-aws" {
    version = "^0.10"
  }
  mod "github.com/turbot/flowpipe-mod-gcp" {
    version = "*"
  }
}
```
