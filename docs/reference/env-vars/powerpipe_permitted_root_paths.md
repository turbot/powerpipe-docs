---
title: POWERPIPE_PERMITTED_ROOT_PATHS
---

# POWERPIPE_PERMITTED_ROOT_PATHS
Sets the folders that mods can use files from, as a comma-separated list of absolute paths. When it is set, Powerpipe stops with an error when a mod or a [connection](/docs/reference/config-files/connection) names a file outside these folders. When it is not set, there is no limit.

This setting needs Powerpipe v1.9.0 or later. An earlier version ignores it.

Set it when Powerpipe runs mods that you do not trust, for example on a shared server. List the folder of each mod that you run. A mod's dependency mods are in its own folder, so they are covered too. A mod whose folder is not in the list still loads, but it cannot use files from its own folder.

When the setting is set:

- The HCL file functions, such as `file()` and `fileset()`, read only inside the folders.
- A DuckDB or SQLite `database` in a mod, a resource, a `require` block, a variable, a connection or a workspace must be a file inside the folders, or `:memory:`. It cannot have options, such as `?access_mode=read_only`.
- A Postgres `database` can name its `sslcert`, `sslkey`, `sslrootcert` and `passfile` only inside the folders. It cannot use `service` or `servicefile`.
- A MySQL `database` cannot use `allowAllFiles`.
- `duckdbinit://` values and Tailpipe connections are not allowed.
- A workspace's `mod_location`, and its `snapshot_location` when it is a folder, must be inside the folders. Powerpipe checks every workspace in your config files, not only the one you use, so one workspace outside the folders stops every command.
- SQL that runs in DuckDB can read and write files only inside the folders. It cannot read URLs, such as `https://` or `s3://`, and it cannot install or load extensions. DuckDB keeps its temporary files in a `.duckdb_tmp` folder inside the first folder in the list that it can write to.
- Powerpipe does not open a DuckDB database while a symlink in one of the folders leads outside them.
- SQLite cannot `ATTACH` a file outside the folders, or a name that it cannot check, and cannot run `VACUUM`.
- Powerpipe does not load or install a mod that holds a symlink to a path outside the folders. A `require` block's local `path` must be inside the folders.

The setting does not limit SQL that runs on a Postgres, MySQL or Steampipe server. That SQL runs with the permissions of the server's own database user.

The setting does not check a connection string that you give with `--database` or [POWERPIPE_DATABASE](/docs/reference/env-vars/powerpipe_database). It does check a connection name that you give there, such as `--database connection.duckdb.local`, and a database that you set with `--var`. SQL that runs in DuckDB stays inside the folders in every case.

Each folder must be an absolute path. If one is not, Powerpipe stops when it starts, with an error that names it.

## Usage

Let mods use files only in the `/srv/mods/aws_compliance` folder:

```bash
export POWERPIPE_PERMITTED_ROOT_PATHS=/srv/mods/aws_compliance
```

Also let them use the DuckDB files in `/srv/data`:

```bash
export POWERPIPE_PERMITTED_ROOT_PATHS=/srv/mods/aws_compliance,/srv/data
```

A mod that reads a file outside these folders stops with an error such as:

```
file("../secrets.txt"): path is outside the folders POWERPIPE_PERMITTED_ROOT_PATHS permits.
```

Remove the limit:

```bash
unset POWERPIPE_PERMITTED_ROOT_PATHS
```
