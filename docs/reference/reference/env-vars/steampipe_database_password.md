---
title: STEAMPIPE_DATABASE_PASSWORD
sidebar_label: STEAMPIPE_DATABASE_PASSWORD
---


# STEAMPIPE_DATABASE_PASSWORD

Sets the powerpipe database password for this session.  By default, powerpipe creates a random, unique password for the `powerpipe` user.  To use a different password, set the `STEAMPIPE_DATABASE_PASSWORD` variable and start the powerpipe service.

Note the following:
- Powerpipe sets the `powerpipe` user password when the database starts, thus this variable must be set when the powerpipe service starts.
- If the `--database-password` is passed to `powerpipe service start`, it will override this environment variable.
- Setting `STEAMPIPE_DATABASE_PASSWORD` (or passing the `--database-password` argument) sets the password for the current service instance only - it does not permanently change the powerpipe password.  You can permanently change the default password by editing the `~/.powerpipe/internal/.passwd`.  Deleting this file will result in a new random password being generated the next time powerpipe starts.
- Both `powerpipe` and `root` can login from the local host ([`samehost` in the `pg_hba.conf` file](https://www.postgresql.org/docs/14/auth-pg-hba-conf.html)) without a password, regardless of the `STEAMPIPE_DATABASE_PASSWORD` value.


## Usage 
Start the powerpipe service with a custom password:

```bash
export STEAMPIPE_DATABASE_PASSWORD=MyPassword123
powerpipe service start
```

