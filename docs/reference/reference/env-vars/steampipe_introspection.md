---
title: STEAMPIPE_INTROSPECTION
sidebar_label: STEAMPIPE_INTROSPECTION
---

# STEAMPIPE_INTROSPECTION

Powerpipe can create a set of introspection tables that allow you to query the mod resources in the workspace.  For performance reasons, introspection is disabled by default, however you can enable it by setting the `STEAMPIPE_INTROSPECTION` environment variable.

Once enabled, you can query the introspection tables.  For example, you can list all the benchmarks in the workspace:

```
> select resource_name from powerpipe_benchmark order by resource_name
+----------------------+
| resource_name        |
+----------------------+
| cis_v130             |
| cis_v130_1           |
| cis_v130_2           |
| cis_v130_2_1         |
| cis_v130_2_2         |
| cis_v130_3           |
| cis_v130_4           |
| cis_v130_5           |
| pci_v321             |
| pci_v321_autoscaling |
| pci_v321_cloudtrail  |
| pci_v321_kms         |
+----------------------+
```



<!--
generate with:
select table_name from information_schema.tables where table_name like 'powerpipe_%' and table_type = 'LOCAL TEMPORARY'
-->
When introspection is enabled, the following tables are available to query:
- `powerpipe_benchmark`
- `powerpipe_control`
- `powerpipe_dashboard`
- `powerpipe_dashboard_card`
- `powerpipe_dashboard_chart`
- `powerpipe_dashboard_container`
- `powerpipe_dashboard_flow`
- `powerpipe_dashboard_graph`
- `powerpipe_dashboard_hierarchy`
- `powerpipe_dashboard_image`
- `powerpipe_dashboard_input`
- `powerpipe_dashboard_table`
- `powerpipe_dashboard_text`
- `powerpipe_mod`
- `powerpipe_query`
- `powerpipe_reference`
- `powerpipe_variable`



## Usage 


Enable introspection data

```bash
export STEAMPIPE_INTROSPECTION=info
```


Disable introspection data (the default):

```bash
unset STEAMPIPE_INTROSPECTION

```