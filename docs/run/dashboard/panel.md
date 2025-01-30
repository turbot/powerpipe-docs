---
title: Panel View
---


# Panel View

Most Powerpipe dashboard elements (chart, card, table, etc) support a **Panel View** that allows you to enlarge the element, inspect its definition, and easily export its data.  

To see the panel view, hover over the element and the expand button (4 opposing arrows) will appear at the top of the chart or table:   

![](/images/docs/cost_chart_with_expander.png)


Click the expand button to enter the panel view.

## Preview

The **Preview** pane displays the element in an enlarged view, allowing you to inspect details that may be difficult to discern in the dashboard view.

![](/images/docs/cost_chart_preview.png)


## Definition

The **Definition** pane displays the HCL definition for the element. 

![](/images/docs/cost_chart_definition.png)


## Query

The **Query** pane displays the SQL query used by the element.  You can cut and paste this query into a SQL query editor (including the Powerpipe query shell) to run or save it.

![](/images/docs/cost_chart_query.png)

## Data

The **Data** pane displays a table of the results of the SQL query used by the element.  For a `table`, this will also include any hidden columns.  To download the data to CSV, click the **Download** button.

![](/images/docs/cost_chart_data.png)

