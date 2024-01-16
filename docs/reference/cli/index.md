---
title: Powerpipe CLI
sidebar_label: Powerpipe CLI
---

# Powerpipe CLI

## Sub-Commands

| Command | Description
|-|-
| [powerpipe help](reference/cli/help)      | Help about any command
| [powerpipe mod](reference/cli/mod)        | Powerpipe mod management
| [powerpipe pipeline](reference/cli/pipeline) | List, view, and run Powerpipe pipelines
| [powerpipe process](reference/cli/process) | List and view Powerpipe processes
| [powerpipe server](reference/cli/server)  | Run Powerpipe server, including triggers and integrations
| [powerpipe trigger](reference/cli/pipeline) | List and view Powerpipe triggers



<!--

| [powerpipe variable](reference/cli/variable)| Powerpipe variable management

| [powerpipe login](reference/cli/login)    | Log in to Powerpipe CLoud

| [powerpipe completion](reference/cli/completion)| Generate the autocompletion script for the specified shell

-->
## Global Flags

<table>
  <tr> 
    <th> Flag </th> 
    <th> Description </th> 
  </tr>

  <tr> 
    <td nowrap="true"> <inlineCode>--config-path</inlineCode> </td> 
    <td>  
    Sets the search path for <a href = "/docs/reference/config-files/index">configuration files</a>. This argument accepts a colon-separated list of directories.  All configuration files (<inlineCode>*.fpc</inlineCode>) will be loaded from each path, with decreasing precedence.  The default is <inlineCode>.:$POWERPIPE_INSTALL_DIR/config</inlineCode> (<inlineCode>.:~/.powerpipe/config</inlineCode>).  This allows you to manage your <a href="/docs/reference/config-files/workspace"> workspaces </a> and <a href="/docs/reference/config-files/credential/index">credentials</a> centrally in the <inlineCode>~/.powerpipe/config</inlineCode> directory, but override them in the working directory / mod location if desired.
    </td> 
  </tr>

  <tr> 
    <td nowrap="true"> <inlineCode>-h</inlineCode>, <inlineCode>--help</inlineCode> </td> 
    <td>  Help for Powerpipe. </td> 
  </tr>
                  
  <tr> 
    <td nowrap="true"> <inlineCode>--host</inlineCode> </td> 
    <td> Run the command against a local or remote server instance.  You may specify the full host and port (e.g. <inlineCode>--host https://powerpipe.my-org.com:7103</inlineCode>), or use the keyword <inlineCode>local</inlineCode> to connect to the local server instance as a shortcut for <inlineCode>https://localhost:7103</inlineCode> (e.g. <inlineCode>--host local</inlineCode>) </td> 
  </tr>

  <tr> 
    <td nowrap="true">  <inlineCode>--insecure</inlineCode> </td> 
    <td> Ignore any TLS certificate errors and warnings when connecting to a Powerpipe API host. </td> 
  </tr>



  <tr> 
    <td nowrap="true"> <inlineCode>--mod-location</inlineCode>  </td> 
    <td> Sets the Powerpipe workspace working directory.  If not specified, the workspace directory will be set to the current working directory.  See <a href="/docs/reference/env-vars/powerpipe_mod_location">POWERPIPE_MOD_LOCATION</a> for details. </td>
  </tr>

   <tr> 
    <td nowrap="true">  <inlineCode>--output</inlineCode> </td> 
    <td>  Select a console output format: <inlineCode>pretty</inlineCode>, <inlineCode>plain</inlineCode>, <inlineCode>yaml</inlineCode> or <inlineCode>json</inlineCode> (default <inlineCode>pretty</inlineCode>). </td>
  </tr>

  <tr> 
    <td nowrap="true"> <inlineCode>-v</inlineCode>, <inlineCode>--version</inlineCode>  </td> 
    <td>  Display Powerpipe version. </td> 
  </tr>

  <tr> 
    <td nowrap="true"> <inlineCode>--workspace	</inlineCode>  </td> 
    <td>  Sets the Powerpipe workspace profile. If not specified, the default workspace will be used if it exists. See <a href="/docs/reference/env-vars/powerpipe_workspace">POWERPIPE_WORKSPACE</a> for details. </td> 
  </tr>


  



</table>



<!--

  <tr> 
    <td nowrap="true"> <inlineCode>--cloud-host</inlineCode>  </td> 
    <td>  Sets the Powerpipe Cloud host used when connecting to Powerpipe Cloud workspaces.  See <a href="/docs/reference/env-vars/powerpipe_cloud_host">STEAMPIPE_CLOUD_HOST</a> for details. </td>
  </tr>

  <tr> 
    <td nowrap="true"> <inlineCode>--cloud-token</inlineCode>  </td> 
    <td>  Sets the Powerpipe Cloud authentication token used when connecting to Powerpipe Cloud workspaces.  See <a href="/docs/reference/env-vars/powerpipe_cloud_token">STEAMPIPE_CLOUD_TOKEN</a> for details. </td>
  </tr>



  <tr> 
    <td nowrap="true"> <inlineCode>--workspace</inlineCode>  </td> 
    <td>  Sets the Powerpipe <a href="/docs/run/workspaces"> workspace profile</a>.  If not specified, the <inlineCode>default</inlineCode> workspace will be used if it exists.  See <a href="/docs/reference/env-vars/powerpipe_workspace">STEAMPIPE_WORKSPACE</a> for details.</td>
  </tr>


  <tr> 
    <td nowrap="true"> <inlineCode>--workspace-database</inlineCode>  </td> 
    <td>  Sets the database that Powerpipe will connect to. This can be <inlineCode>local</inlineCode> (the default) or a remote Powerpipe Cloud database.  See <a href="/docs/reference/env-vars/powerpipe_workspace_database">STEAMPIPE_WORKSPACE_DATABASE</a> for details. </td>
  </tr>

-->


---

