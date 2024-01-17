---
title: POWERPIPE_WORKSPACE_DATABASE
sidebar_label: POWERPIPE_WORKSPACE_DATABASE
---


# POWERPIPE_WORKSPACE_DATABASE
Sets the database that Powerpipe will connect to. By default, Powerpipe will use the locally installed database Steampipe database.  


## Usage 
Use a Powerpipe cloud remote database:
```bash
export POWERPIPE_CLOUD_TOKEN=tpt_c6f5tmpe4mv9appio5rg_3jz0a8fakekeyf8ng72qr646
export POWERPIPE_WORKSPACE_DATABASE=acme/prod
```

Use a remote postgres database via connection string:
```bash
export POWERPIPE_WORKSPACE_DATABASE='postgresql://myusername:mypassword@acme-prod.apse1.db.cloud.turbot.io:9193/aaa000'
```

Use a MySQL database via connection string:
```bash
export POWERPIPE_WORKSPACE_DATABASE='mysql://root:my_pass@tcp(myhost.com)/mydb'
```

Use a SQLite database via connection string:
```bash
export POWERPIPE_WORKSPACE_DATABASE='sqlite://./my_sqlite_db.db'
```

Use a DuckDB database via connection string:
```bash
export POWERPIPE_WORKSPACE_DATABASE='duckdb://./my_ducks.db'
```




