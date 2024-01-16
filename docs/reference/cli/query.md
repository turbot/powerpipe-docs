---
title: powerpipe query
sidebar_label: powerpipe query
---


# powerpipe query

blah blah blah


## Usage
```bash
powerpipe query
```

## Flags

| Flag | Description
|-|-
| `--listen string`   | Accept connections from `local` (localhost only) or `network` (all interfaces / IP addresses) (default `network`).
| `--port int`        | Web server port (default `7103`).
| `--var string=string` | Specify the value of a variable.  Multiple `--var` arguments may be passed. 
| `--var-file string`| Specify a `.fpvar` file containing variable values.
| `--watch`             | Watch mod files for changes when running `powerpipe server` (default `true`).

## Examples

Start Powerpipe in server mode:
```bash
powerpipe server
```

Start Powerpipe on port 7104
```bash
powerpipe server --port 7104
```

Start Powerpipe on `localhost` only
```bash
powerpipe server  --listen local
```

Start Powerpipe using settings from a workspace
```bash
powerpipe server --workspace my_powerpipe_server
```

Start Powerpipe in server mode but turn off file watching:
```bash
powerpipe server --watch=false
```

