---
title: powerpipe server
sidebar_label: powerpipe server
---


# powerpipe server

Run the Powerpipe server, including the dashboard server and the API.  Powerpipe server runs in the foreground; Press `Ctrl-C` to exit.


## Usage
```bash
powerpipe server
```

## Flags

| Flag | Description
|-|-
|  `--dashboard-timeout int` | Set the dashboard execution timeout, in seconds. The default is `0` (no timeout).
|  `--database`         |  Sets the [database that Powerpipe will connect to](/docs/run#selecting-a-database). This defaults to the local Steampipe database, but can be any PostgreSQL, MySQL, DuckDB, or SQLite database. See [POWERPIPE_DATABASE](/docs/reference/env-vars/powerpipe_database) for details.
| `--listen string`   | Accept connections from `local` (localhost only) or `network` (all interfaces / IP addresses) (default `network`).
| `--port int`        | Web server port (default `9033`).
| `--var string=string` | Specify the value of a variable.  Multiple `--var` arguments may be passed. 
| `--var-file string`| Specify a `.ppvar` file containing variable values.
| `--watch`             | Watch mod files for changes when running `powerpipe server` (default `true`).


## Examples

Start Powerpipe in server mode:
```bash
powerpipe server
```

Start Powerpipe on port 9195
```bash
powerpipe server --port 9195
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

<!--
TO DO
The value takes the form of a comma-separated list of host names and/or numeric IP addresses. The special entry * corresponds to all available IP interfaces. The entry 0.0.0.0 allows listening for all IPv4 addresses and :: allows listening for all IPv6 addresses. If the list is empty, the server does not listen on any IP interface at all, in which case only Unix-domain sockets can be used to connect to it

# postgres style
powerpipe listen --port 9033 --addresses '*'       # all interfaces
powerpipe listen --port 9033 --addresses 0.0.0.0   # all ipv4 interfaces
powerpipe listen --port 9033 --addresses ::        # all ipv6 interfaces
powerpipe listen --port 9033 --addresses localhost # loopback only
powerpipe listen --port 9033 --addresses 10.0.0.1,127.0.0.1,192.168.0.1 # specific addresses
-->