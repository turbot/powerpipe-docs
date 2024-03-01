---
title: Change the Search Path
sidebar_label: Change the Search Path
---

# Change the Search Path

Many Powerpipe mods use [Steampipe](http://steampipe.io) as the database.  Steampipe leverages PostgreSQL foreign data wrappers to provide an SQL interface to external services and systems. 

Steampipe allows you to create a [connection](https://steampipe.io/docs/managing/connections) for each data source, and aggregate multiple connections for a given type using an [aggregator](https://steampipe.io/docs/managing/connections#using-aggregators).  For example, you may have a **connection** for each of your AWS accounts and a single **aggregator** to query all accounts.

Steampipe creates a Postgres schema for each Steampipe connection and aggregator.  Powerpipe mods are written using unqualified names in the SQL queries.  As a result, they are resolved using the Postgres [search path](https://steampipe.io/docs/guides/search-path). You can effectively switch which connection to target by changing the search path.

When running Powerpipe from the command line, you can pass the `--search-path` or `--search-path-prefix` to [target a specific schema](/docs/run#targeting-specific-schemas--connections-postgressteampipe).  

When running a dashboard, you can change the search path on the fly!  At the top of the page, click the **Search Path** button.  


![](/images/docs/search_path.png)


Select a connection from the list.  Click **Add** to add as many connections as you like, and drag and drop them with the handles (6 dots to the right of the connection name) to reorder them.  When you are done, click **Save**.  The dashboard will run again, using the new search path.

