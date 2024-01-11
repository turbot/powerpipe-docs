---
title: schedule
sidebar_label: schedule
---

# schedule

The `schedule` trigger runs a pipeline on the given schedule. The schedule may be a named interval:

```hcl
trigger "schedule" "my_hourly_trigger" {
    schedule = "hourly"

    pipeline = pipeline.twitter_mentions_to_slack
    args = {
        query = "(steampipe OR steampipe.io OR github.com/turbot) -tomathy -GutturalSteve -\"steampipe alley\""
    }

}
```

or a custom `cron` schedule:

```hcl
trigger "schedule" "my_cron_trigger" {
    schedule = "5 * * * *"

    pipeline = pipeline.twitter_mentions_to_slack
    args = {
        search_string = "(steampipe OR steampipe.io OR github.com/turbot)"
    }
}
```

##### Arguments

| Argument        | Type    | Optional?  | Description
|-----------------|---------|------------|-----------------
| `args`	        | Map	    | Optional	  | A map of arguments to pass to the pipeline.
| `description`   |  String | Optional   | A string containing a short description of the step. 
| `documentation` | String | Optional | A markdown string containing a long form description, used as documentation for the mod on hub.flowpipe.io. 
| `pipeline`      | Pipeline Reference | Optional | A reference to a `pipeline` resource to start when this trigger runs.  
| `schedule`      | String  | Required    | Schedule to run the query. This may be a named interval (`hourly`, `daily`, `weekly`) or a custom schedule in cron syntax
| `tags` | Map | Optional | A map of key:value metadata for the mod, used to categorize, search, and filter.   
| `title`         | String  | Optional | Display title for the step.
