---
title: powerpipe login
sidebar_label: powerpipe login
---


# powerpipe login
Log in to [Turbot Pipes](https://turbot.com/pipes/docs).

The Powerpipe CLI can interact with Turbot Pipes to run pipelines in a remote cloud instance. These capabilities require authenticating to Turbot Pipes.  The `powerpipe login` command launches an interactive process for logging in and obtaining a temporary (30 day) token. The token is written to `~/.powerpipe/internal/{cloud host}.sptt`.

## Usage
```bash
powerpipe login
```

## Examples

Login to `pipes.turbot.com`:

```bash
powerpipe login
```


The `powerpipe login` command will launch your web browser to continue the login process. Verify the request.






---
title: powerpipe login
sidebar_label: powerpipe login
---


# powerpipe login
Log in to [Turbot Pipes](https://turbot.com/pipes/docs).

The Powerpipe CLI can interact with Turbot Pipes to run queries, benchmarks, and dashboards against a remote cloud database, and to save and share snapshots. These capabilities require authenticating to Turbot Pipes.  The `powerpipe login` command launches an interactive process for logging in and obtaining a temporary (30 day) token. The token is written to `~/.powerpipe/internal/{cloud host}.tptt`.

## Usage
```bash
powerpipe login
```

## Flags:

<table>
  <tr> 
    <th> Argument </th> 
    <th> Description </th> 
  </tr>
  <tr> 
    <td nowrap="true"> <inlineCode>--cloud-host</inlineCode> </td> 
    <td>  Sets the Turbot Pipes host used when connecting to Turbot Pipes workspaces. See <a href="reference/env-vars/powerpipe_cloud_host">STEAMPIPE_CLOUD_HOST</a> for details.</td> 
  </tr>

  <tr> 
    <td nowrap="true"> <inlineCode>--cloud-token</inlineCode> </td> 
    <td>  Sets the Turbot Pipes authentication token used when connecting to Turbot Pipes workspaces. See <a href="reference/env-vars/powerpipe_cloud_token">STEAMPIPE_CLOUD_TOKEN</a> for details.</td> 
  </tr>
</table>

## Examples

Login to `pipes.turbot.com`:

```bash
powerpipe login
```


The `powerpipe login` command will launch your web browser to continue the login process. Verify the request.



<img src="/images/docs/powerpipe-login/powerpipe-login-1.png" width="100%" />



After you have verified the request, the browser will display a verification code.   
<img src="/images/docs/powerpipe-login/powerpipe-login-2.png" width="100%" />

Paste the code into the cli and hit enter to complete the login process:

```bash
$ powerpipe login
Verify login at https://pipes.turbot.com/login/token?r=tpttr_cdckfake6ap10t9dak0g_3u2k9hfake46g4o4wym7h8hw
Enter verification code: 745278
Login successful for user johnsmyth
```
