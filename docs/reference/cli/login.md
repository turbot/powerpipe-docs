---
title: powerpipe login
---


# powerpipe login
Log in to [Turbot Pipes](https://turbot.com/pipes/docs).

The Powerpipe CLI can interact with Turbot Pipes to run pipelines in a remote cloud instance. These capabilities require authenticating to Turbot Pipes.  The `powerpipe login` command launches an interactive process for logging in and obtaining a temporary (30-day) token. The token is written to the [PIPES_INSTALL_DIR](/docs/reference/env-vars/pipes_install_dir) and defaults to `~/.pipes/internal/{cloud host}.pptt`.

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
    <td nowrap="true"> <inlineCode>--pipes-host</inlineCode> </td> 
    <td>  Sets the Turbot Pipes host used when connecting to Turbot Pipes workspaces. See <a href="reference/env-vars/pipes_host">PIPES_HOST</a> for details.</td> 
  </tr>

</table>

## Examples

Login to `pipes.turbot.com`:

```bash
powerpipe login
```


The `powerpipe login` command will launch your web browser to continue the login process. Verify the request.

![](/images/docs/powerpipe-login/powerpipe-login-1.png)


After you have verified the request, the browser will display a verification code. 

![](/images/docs/powerpipe-login/powerpipe-login-2.png)

Paste the code into the CLI and hit enter to complete the login process:

```bash
$ powerpipe login
Verify login at https://pipes.turbot.com/login/token?r=tpttr_cdckfake6ap10t9dak0g_3u2k9hfake46g4o4wym7h8hw
Enter verification code: 745279
Login successful for user johnsmyth
```
