---
title: POWERPIPE_DATABASE
---


# POWERPIPE_DATABASE


> [!IMPORTANT]
> ***As of Powerpipe 1.0.0, `POWERPIPE_DATABASE` has been deprecated and will be removed in a future version of Powerpipe. See [Setting the Database](/docs/build/mod-database) for the new syntax.***


Sets the database that Powerpipe will connect to. By default, Powerpipe will use a locally installed Steampipe database.  


## Usage 
Use a Powerpipe cloud remote database:
```bash
export PIPES_TOKEN=tpt_c6f5tmpe4mv9appio5rg_3jz0a8fakekeyf8ng72qr646
export POWERPIPE_DATABASE=acme/prod
```

Use a remote PostgreSQL database via connection string:
```bash
export POWERPIPE_DATABASE='postgresql://myusername:mypassword@acme-prod.apse1.db.cloud.turbot.io:9193/aaa000'
```

Use a MySQL database via connection string:
```bash
export POWERPIPE_DATABASE='mysql://root:my_pass@tcp(myhost.com)/mydb'
```

Use a SQLite database via connection string:
```bash
export POWERPIPE_DATABASE='sqlite:./my_sqlite_db.db'
```

Use a DuckDB database via connection string:
```bash
export POWERPIPE_DATABASE='duckdb:./my_ducks.db'
```




