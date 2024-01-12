---
title: powerpipe plugin
sidebar_label: powerpipe plugin
---

# powerpipe plugin
Powerpipe plugin management.

Plugins extend Powerpipe to work with many different services and providers. Find plugins using the public registry at [hub.powerpipe.io](https://hub.powerpipe.io).


## Usage
```bash
powerpipe plugin [command]
```

## Available Commands:

| Command | Description
|-|-
| `install`     | Install one or more plugins
| `list`        | List currently installed plugins
| `uninstall`   | Uninstall a plugin
| `update `     | Update one or more plugins

<table>
  <thead>
    <tr>
      <th nowrap="true">Flag</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td nowrap="true"><inlineCode>--all</inlineCode></td>
      <td>Applies only to <inlineCode>plugin update</inlineCode>, updates ALL installed plugins.</td>
    </tr>
    <tr>
      <td nowrap="true"><inlineCode>--progress</inlineCode></td>
      <td>Enable or disable progress information. By default, progress information is shown - set <inlineCode>--progress=false</inlineCode> to hide the progress bar. Applies only to <inlineCode>plugin install</inlineCode> and <inlineCode>plugin update</inlineCode>.</td>
    </tr>
      <tr>
      <td nowrap="true"><inlineCode>--skip-config </inlineCode></td>
      <td>Applies only to <inlineCode>plugin install</inlineCode>,  skip creating the default config file for plugin.</td>
    </tr>
  </tbody>
</table>

## Examples

Install or update a plugin:
```bash
powerpipe plugin install aws
```

Install a specific version of a plugin:
```bash
powerpipe plugin install aws@0.107.0
```

Install all missing plugins that specified in configuration files. Do not download their default configuration files:

```bash
powerpipe plugin install --skip-config
```

List installed plugins:
```bash
powerpipe plugin list
```

Uninstall a plugin:
```bash
powerpipe plugin uninstall dmi/paper
```

Update all plugins to the latest in the installed stream:
```bash
powerpipe plugin update --all
```

Update the aws plugin to the latest in the 0.1 minor stream:
```bash
powerpipe plugin update aws@0.1
```

Update all plugins to the latest and hide the progress bar:
```bash
powerpipe plugin update --all --progress=false
```
