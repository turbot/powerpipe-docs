---
title: mod
sidebar_label: mod
---


# mod
Every mod must contain a `mod.pp` file with a single `mod` block.

The `mod` block contains metadata for the mod (including metadata used in the hub site and social media), as well as dependency data.  A mod author may edit the mod block directly, but Powerpipe will **also** edit the file, adding, removing and modifying dependencies in the file when users add and remove mods via the [`powerpipe mod` commands](/docs/reference/cli/mod).  For this reason, it is recommended that the `mod.pp` *only* contain a `mod` block; do not add other mod resources (`query`, `control`, `dashboard`, etc) to this file.

The block name (`aws_cis` in the example) is the mod name.  Mod names use lower_snake_case. They may contain lowercase chars, numbers or underscores, and must start with a letter.


## Example Usage

```hcl
mod "aws_cis" { 
  # hub metadata
  title          = "AWS CIS"
  description    = "AWS CIS Reporting and remediation"
  color          = "#FF9900"
  documentation  = file("./aws_cis_docs.md")
  icon           = "/images/plugins/turbot/aws.svg"
  categories         = ["Public Cloud", "AWS"]

  opengraph {
    title         = "Powerpipe Mod for AWS CIS"
    description   = "CIS reports, queries, and actions for AWS. Open source CLI. No DB required."
  }

  require {
    powerpipe {
      min_version = "0.20.0"
    }

    plugin "aws" {
      min_version = "0.86.0"
    }

    plugin "gcp" {
      min_version = "0.29.0"
    }

    mod "github.com/turbot/steampipe-mod-aws-compliance" {
      version = "^0.10"
    }
    mod "github.com/turbot/steampipe-mod-gcp-compliance" {
      version = "*"
    }
  }
}
```

## Argument Reference

| Name | Type | Required? | Description
|-|-|-|-
| `categories` | List(String) | Optional | A list of labels, used to categorize mods (such as on the Powerpipe Hub).
| `color` | String |Optional |  A hexadecimal RGB value to use as the color scheme for the mod on hub.powerpipe.io.  
| `description` |  String | Optional | A string containing a short description. 
| `documentation` | String (Markdown)| Optional | A markdown string containing a long form description, used as documentation for the mod on hub.powerpipe.io. 
| `icon` |  String | Optional | The url of an icon to use for the mod on hub.powerpipe.io.
| `opengraph` |  Block | Optional | Block of metadata for use in social media applications that support [Opengraph](#opengraph) metadata.
| `require` | Block | Optional | A block that specifies one or more [mod dependencies](#require).
| `tags` | Map | Optional | A map of key:value metadata for the mod, used to categorize, search, and filter.   
| `title` | String | Optional | The display title of the mod.


### opengraph
The `opengraph` block is an optional block of metadata for use in social media applications that support [Opengraph](https://ogp.me/) metadata.

| Name | Type| Description
|-|-|-
| `description` | String | The opengraph description (`og:description`) of the mod, for use in social media applications.
| `title` | String | The opengraph display title (`og:title`) of the mod, for use in social media applications.

 

### require
A mod may contain a `require` block to specify version dependencies for the Powerpipe CLI, plugins, and mods.  While it is possible to edit this section manually, Powerpipe will also modify it (including reordering and removing comments) when you run a `powerpipe mod` command to install, update, or uninstall a mod.


#### powerpipe
A mod may specify a dependency on the Powerpipe CLI.  Powerpipe will evaluate the dependency when the mod is loaded and will error if the constraint is not met, but it will not install or upgrade the CLI.  A `powerpipe` constraint specifies a *minimum version*, and does not support semver syntax:
```hcl
require {
  powerpipe {
    min_version = "0.20.0"
  }
}
```

#### plugin
A mod may specify a dependency on one or more Steampipe plugins.  Powerpipe will evaluate the dependency when the mod is loaded and will error if the constraint is not met, but it will not install or upgrade the plugin. A `plugin` constraint specifies a *minimum version*, and does not support semver syntax:
```hcl
require {
  plugin "aws" {
    min_version = "0.24.0"
  }
}
```

#### mod
A mod may specify dependencies on other mods.  While you can manually edit the `mod` dependencies in the `mod.pp`, they are more commonly managed by Powerpipe when you install, update, or uninstall mods via the [powerpipe mod commands](/docs/reference/cli/mod).

The mod dependency may specify a `version`.  This can be a [semver](https://semver.org/) or an exact version:

```hcl
require {
  mod "github.com/turbot/steampipe-mod-aws-insights" {
    version = "0.20.0"
  }
  mod "github.com/turbot/steampipe-mod-aws-compliance" {
    version = "^0.10"
  }
  mod "github.com/turbot/steampipe-mod-gcp-compliance" {
    version = "*"
  }
}
```

While in development, it is often easier to use local dependencies or install a mod from a branch or tagged commit.

To install from a branch, specify a `branch`:

```hcl
require {
  mod "github.com/turbot/steampipe-mod-aws-compliance" {
    branch = "issue-779"
  }
}
```

To install from a tagged commit, specify a `tag`:

```hcl
require {
  mod "github.com/turbot/steampipe-mod-aws-compliance" {
    tag = "issue-779"
  }
}
```


To install from a local directory, specify a `path` instead of a `version`:

```hcl
require {
  mod "/Users/jsmyth/src/steampipe-mod-aws-insights" {
    path = "/Users/jsmyth/src/steampipe-mod-aws-insights"
  }
}
```

Note that for a given dependency, you must specify *exactly one* constraint (`version`, `tag`, `branch` or `path`).


You may pass `args` to set variables defined in the dependency mods.  You can pass hard-coded values (literals), but it is more common to define variables in your mod that encapsulate the variables  and optionality of your dependencies:


```hcl
variable "common_dimensions" {
  type        = list(string)
  description = "A list of common dimensions to add to each control."
  default     = [ "account_id", "region" ]
}

variable "tag_dimensions" {
  type        = list(string)
  description = "A list of tags to add as dimensions to each control."
  default     = []
}

mod "aws_well_architected" {
  require {
    mod "github.com/turbot/steampipe-mod-aws-compliance" {
      version = "^0.63.0"
      args = {
        common_dimensions = var.common_dimensions,
        tag_dimensions    = var.tag_dimensions
      }
    }
  }
}

```


You may also override the [default database](/docs/run#selecting-a-database) and [search_path / search_path_prefix](/docs/run/benchmark#targeting-specific-connections-postgres-only) for the dependency mod if you want.  Note that if you override the `database` in the mod `require` block, the setting will take precedence; the `STEAMPIPE_DATABASE` variable, `database` workspace argument and `--database` command line argument will be ignored for resources in the dependency mod. Likewise, overriding the `database`, `search_path` or `search_path_prefix` will cause the `search_path` and `search_path_prefix`	workspaces arguments and `--search-path` and `--search-path-prefix` CLI arguments to be ignored for the dependency mod.

```hcl
variable "duckdb_database" {
  default = "duckdb:/home/ducks/mallard.db"
}

variable "flowpipe_database" {
  default = "sqlite:/./my_mod/flowpipe.db"
}

mod "local" {
  require {
    mod "github.com/turbot/my-duckdb-mod" {
      version     = ">=0.1.0"
      database    = var.duckdb_database
      args = {
        foo = "bar"
      }
    }

    mod "github.com/turbot/my-flowpipe-mod" {
      version   = ">=0.1.0"
      database  = var.flowpipe_database,
    }

    mod "github.com/turbot/steampipe-mod-aws-compliance" {
      version            = ">=0.66.0"
      search_path_prefix = "aws_01"
    }
  }
}
```