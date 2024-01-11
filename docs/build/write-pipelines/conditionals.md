---
title: Conditional Execution
sidebar_label: Conditional Execution
---
# Conditional Execution

You can conditionally execute a step using the `if` argument:

```hcl
pipeline "notify_unencrypted" {
  step "query" "unencrypted_vols" {
    connection_string = "postgres://steampipe@localhost:9193/steampipe"
    sql               = "select arn from aws_ebs_volume where not encrypted"
  }

  step "http" "notify_slack" {
    if     = length(step.query.unencrypted_vols.rows) > 0
    url    = "https://hooks.slack.com/services/T04THIS54LQ/B04ISH1B2GM/vIakTJfqFAKET7M14g5H32w8"
    method = "post"
    
    request_body = jsonencode({
      text = "Unencrypted Volumes: ${join(", ", step.query.unencrypted_vols.rows[*].arn)}"
    })
  }
}
```

Flowpipe does not provide `else` or `case` arguments to `step`, but you can mimic the behavior by using different conditions in multiple steps:

```hcl
pipeline "notify_unencrypted" {
  step "query" "unencrypted_vols" {
    connection_string = "postgres://steampipe@localhost:9193/steampipe"
    sql               = "select arn from aws_ebs_volume where not encrypted"
  }

  step "http" "notify_slack" {
    if     = length(step.query.unencrypted_vols.rows) > 0
    url    = "https://hooks.slack.com/services/T04THIS54LQ/B04ISH1B2GM/vIakTJfqFAKET7M14g5H32w8"
    method = "post"
    
    request_body = jsonencode({
      text = "There are ${length(step.query.unencrypted_vols.rows)} unencrypted volumes!"
    })
  }

  step "http" "notify_slack_all_encrypted" {
    if     = length(step.query.unencrypted_vols.rows) == 0
    url    = "https://hooks.slack.com/services/T04THIS54LQ/B04ISH1B2GM/vIakTJfqFAKET7M14g5H32w8"
    method = "post"
    
    request_body = jsonencode({
      text = "All Volumes are encrypted!"
    })
  }
}
```

In this instance, you could accomplish the same thing using [conditional expressions](https://developer.hashicorp.com/terraform/language/expressions/conditionals):


```hcl
pipeline "notify_unencrypted" {
  step "query" "unencrypted_vols" {
    connection_string = "postgres://steampipe@localhost:9193/steampipe"
    sql               = "select arn from aws_ebs_volume where not encrypted"
  }

  step "http" "notify_slack" {
    url    = "https://hooks.slack.com/services/T04THIS54LQ/B04ISH1B2GM/vIakTJfqFAKET7M14g5H32w8"
    method = "post"
    
    request_body = (length(step.query.unencrypted_vols.rows) > 0
      ? jsonencode({ text = "There are ${length(step.query.unencrypted_vols.rows)} unencrypted volumes!" })
      : jsonencode({ text = "All Volumes are encrypted!" })
    )
  }
}
```

or string [directives](https://developer.hashicorp.com/terraform/language/expressions/strings#directives):


```hcl
pipeline "notify_unencrypted" {
  step "query" "unencrypted_vols" {
    connection_string = "postgres://steampipe@localhost:9193/steampipe"
    sql               = "select arn from aws_ebs_volume where not encrypted"
  }

  step "http" "notify_slack" {
    url    = "https://hooks.slack.com/services/T04THIS54LQ/B04ISH1B2GM/vIakTJfqFAKET7M14g5H32w8"
    method = "post"
    
    request_body = jsonencode({ text = "%{if length(step.query.unencrypted_vols.rows) > 0}There are ${length(step.query.unencrypted_vols.rows)} unencrypted volumes! %{else} All Volumes are encrypted! %{endif}" })
  }
}
```