---
title: email
sidebar_label: email
---

# email

The `email` step is used to send an email message.


```hcl
pipeline "another_pipe" {

  step "email" "send_it" {
      to                = ["darin@kramerica.com"]
      from              = "elaine@jpetermancatalog.com"
      smtp_username     = "elaine@jpetermancatalog.com"
      smtp_password     = "54678fakeblksdf09"
      host              = "smtp.example.com"
      port              = 587
      subject           = "Order Shipped"
      content_type      = "text/html"
      body              = <<-EOS
        <h1> Your Urban Sombrero is on its way! </h1>

        <h3> Combining the spirit of old Mexico with a little big city panache...</h2

        <p>Thank you for shopping with J Peterman. </p>
      EOS
  }
}

```


## Arguments

| Argument        | Type        | Optional?  | Description
|-----------------|-------------|------------|-----------------
| `to`            | List of String| Required   | A list of recipient email addresses to which 
| `bcc`           | List of String| Optional   | A list of recipient email addresses to which to send the message via bcc.
| `body`          | String      | Optional   | The message body.
| `cc`            | List of String| Optional   | A list of recipient email addresses to which to send the message via cc.
| `content_type`  | String      | Optional   | The content type of the email body, e.g. `text/plain`, `text/html` to send the message.
| `from`          | String      | Optional   | The email address of the sender.
| `host`          | String      | Optional   | The DNS name of the SMTP host to send the mail through.
| `port`          | Number      | Optional   | The TCP port on the SMTP host.
| `sender_name`   | String      | Optional   | A friendly display name of the sender
| `smtp_password` | String      | Optional   | Password to log in to the SMTP server.
| `smtp_username` | String      | Optional   | Username to log in to the SMTP server.
| `subject`       | String      | Optional   | The message subject.

This step also supports the [common step arguments](/docs/flowpipe-hcl/step/index#common-step-arguments) and [attributes](/docs/flowpipe-hcl/step/index#common-step-attributes-read-only).
