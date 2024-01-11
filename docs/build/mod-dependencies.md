---
title: Mod Dependencies
sidebar_label: Mod Dependencies
---

# Mod Dependencies

Flowpipe mods may depend on other mods, allowing you to quickly and easily extend them with additional features and functionality.  You can add, remove, and update your dependencies with the [flowpipe mod command](/docs/reference/cli/mod). 

To add a dependency, run `flowpipe mod install` from the root directory of your mod, specifying the path to the mod's Github repo:

```bash
cd my-mod
flowpipe mod install github.com/turbot/flowpipe-mod-slack
```

This will install the mod into the `.flowpipe` sub-directory, and will add the dependency to your `mod.fp` file:
```hcl
mod "local" {
  title = "my-mod"
  require {
    mod "github.com/turbot/flowpipe-mod-slack" {
      version = "latest"
    }
  }
}
```

You can then create new `.fp` files in your mod that reference the resources in the dependency mods.  You can use a [pipeline step](/docs/flowpipe-hcl/step/pipeline) to run a pipeline in the dependency mod:

```hcl
pipeline "my_pipeline" {

  step "pipeline" "post_message" {
    pipeline = slack.pipeline.post_message
    args = {
      channel = "#general"
      text    = "This is a test message"
    }
  }
}
```

You can run also run a pipeline from a direct dependency from the command line.  Since the pipeline is in a dependency mod, you have to qualify the pipeline reference with the mod name:

```bash
flowpipe pipeline run slack.pipeline.post_message --arg channel="#general" --arg text="This is a test message"
```