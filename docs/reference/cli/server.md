---
title: flowpipe server
sidebar_label: flowpipe server
---


# flowpipe server

Run the Flowpipe server, including triggers, integrations, and the API.  Flowpipe server runs in the foreground; Press `Ctrl-C` to exit.


## Usage
```bash
flowpipe server
```

## Flags

| Flag | Description
|-|-
| `--listen string`   | Accept connections from `local` (localhost only) or `network` (all interfaces / IP addresses) (default `network`).
| `--port int`        | Web server port (default `7103`).
| `--var string=string` | Specify the value of a variable.  Multiple `--var` arguments may be passed. 
| `--var-file string`| Specify a `.fpvar` file containing variable values.
| `--watch`             | Watch mod files for changes when running `flowpipe server` (default `true`).

## Examples

Start Flowpipe in server mode:
```bash
flowpipe server
```

Start Flowpipe on port 7104
```bash
flowpipe server --port 7104
```

Start Flowpipe on `localhost` only
```bash
flowpipe server  --listen local
```

Start Flowpipe using settings from a workspace
```bash
flowpipe server --workspace my_flowpipe_server
```

Start Flowpipe in server mode but turn off file watching:
```bash
flowpipe server --watch=false
```

<!--
TO DO
The value takes the form of a comma-separated list of host names and/or numeric IP addresses. The special entry * corresponds to all available IP interfaces. The entry 0.0.0.0 allows listening for all IPv4 addresses and :: allows listening for all IPv6 addresses. If the list is empty, the server does not listen on any IP interface at all, in which case only Unix-domain sockets can be used to connect to it

# postgres style
flowpipe listen --port 7103 --addresses '*'       # all interfaces
flowpipe listen --port 7103 --addresses 0.0.0.0   # all ipv4 interfaces
flowpipe listen --port 7103 --addresses ::        # all ipv6 interfaces
flowpipe listen --port 7103 --addresses localhost # loopback only
flowpipe listen --port 7103 --addresses 10.0.0.1,127.0.0.1,192.168.0.1 # specific addresses
-->