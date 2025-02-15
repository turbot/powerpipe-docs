---
title: Filter & Group
---


# Filter & group

When a dashboard displays benchmarks, the **Filter & Group** button enables you to filter which controls/detections are shown.

Here we filter the CIS v4.0.0 benchmark to just controls in the **Alarm** state.

![](/run/view/filter-group-controls-1.png)

We can also adjust the hierarchy. For compliance benchmarks the grouping is **Benchmark / Control  / Result**, but here we show only the **Result** group.

![](/run/view/filter-group-controls-2.png)

Similarly, we can filter the Cloudtrail Log Detections benchmark to just detections related to one MITRE ATT&CK id.

![](/run/view/filter-group-detections-1.png)


And again we can adjust the hierarchy. The default is **Benchmark / Detection / Result** but we can flatten to just **Detection / Result**.

![](/run/view/filter-group-detections-2.png)

## Row-level filtering

When a dashboard displays tabular data, you can filter on a per-row basis too. You can include only rows that match the value shown in a column.

![](/run/view/row-level-filter-1.png)

Alternatively, you can exclude raws that match the value shown in a column.

![](/run/view/row-level-filter-2.png)
