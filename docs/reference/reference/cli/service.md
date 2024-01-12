---
title: powerpipe service
sidebar_label: powerpipe service
---


# powerpipe service
Powerpipe service management.

`powerpipe service` allows you to run Powerpipe as a local service, exposing it as a database endpoint for connection from any Postgres-compatible database client.

## Usage
```bash
powerpipe service [command]
```

## Sub-Commands

| Command | Description
|-|-
| `restart` | Restart Powerpipe service
| `start`   | Start Powerpipe in service mode
| `status`  | Status of the Powerpipe service
| `stop`    | Stop Powerpipe service


## Flags

| Flag | Applies to | Description
|-|-|-
| `--dashboard` |  `start` | Start the `dashboard` web server with the database service
| `--dashboard-listen string` | `start` | Accept dashboard connections from: `local` (localhost only) or `network` (open)
| `--dashboard-port int` |  `start` | Dashboard web server port (default `9194`)
| `--database-listen string` |  `start` | Accept database connections from: `local` (localhost only) or `network` (open)
| `--database-password string`  |  `start` |  Set the powerpipe database password for this session.  See [STEAMPIPE_DATABASE_PASSWORD](reference/env-vars/powerpipe_database_password) for additional information
| `--database-port int` | `start` |  Database service port (default 9193)
| `--force` |  `stop`, `restart` | Forces the service to shutdown, releasing all open connections and ports
| `--foreground` |  `start` | Run the service in the foreground
| `--mod-location string` | `start` | Sets the Powerpipe workspace working directory. If not specified, the workspace directory will be set to the current working directory. See <a href="reference/env-vars/powerpipe_mod_location">STEAMPIPE_MOD_LOCATION</a> for details (only applies if '--dashboard' flag is also set)
| `--show-password` |  `start`, `status` | View database password for connecting from another machine (default false)
| `--var stringArray` |  `start` | Specify the value of a variable (only applies if '--dashboard' flag is also set)
| `--var-file strings` |  `start` | Specify an .spvar file containing variable values (only applies if '--dashboard' flag is also set)
| `--all` |  `status` | Bypass the `--install-dir` and print status of all running services




## Examples

Start Powerpipe in the background (service mode):
```bash
powerpipe service start
```

Start Powerpipe on port 9194
```bash
powerpipe service start --database-port 9194
```

Start the Powerpipe service with a custom password:
```bash
powerpipe service start --database-password MyCustomPassword
```


Start Powerpipe on `localhost` only
```bash
powerpipe service start --database-listen local
```

Start Powerpipe with `dashboard`
```bash
powerpipe service start --dashboard
```

Start Powerpipe with `dashboard` running on `localhost` only
```bash
powerpipe service start --dashboard --dashboard-listen local
```

Start Powerpipe with `dashboard` running on port 9195
```bash
powerpipe service start --dashboard --dashboard-port 9195
```

Stop the Powerpipe service:
```bash
powerpipe service stop
```

Forcefully kill all Powerpipe services:
```bash
powerpipe service stop --force
```

View Powerpipe service status:
```bash
powerpipe service status
```

View Powerpipe service status and display the database password:
```bash
powerpipe service status --show-password
```

View status of all running Powerpipe services:
```bash
powerpipe service status --all
```

Restart the Powerpipe service:
```bash
powerpipe service restart
```
