---
title: Flowpipe CLI
sidebar_label: Flowpipe CLI
---

# Flowpipe CLI

## Sub-Commands

| Command | Description
|-|-
| [flowpipe help](reference/cli/help)      | Help about any command
| [flowpipe mod](reference/cli/mod)        | Flowpipe mod management
| [flowpipe pipeline](reference/cli/pipeline) | List, view, and run Flowpipe pipelines
| [flowpipe process](reference/cli/process) | List and view Flowpipe processes
| [flowpipe server](reference/cli/server)  | Run Flowpipe server, including triggers and integrations
| [flowpipe trigger](reference/cli/pipeline) | List and view Flowpipe triggers



<!--

| [flowpipe variable](reference/cli/variable)| Flowpipe variable management

| [flowpipe login](reference/cli/login)    | Log in to Flowpipe CLoud

| [flowpipe completion](reference/cli/completion)| Generate the autocompletion script for the specified shell

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
    Sets the search path for <a href = "/docs/reference/config-files/index">configuration files</a>. This argument accepts a colon-separated list of directories.  All configuration files (<inlineCode>*.fpc</inlineCode>) will be loaded from each path, with decreasing precedence.  The default is <inlineCode>.:$FLOWPIPE_INSTALL_DIR/config</inlineCode> (<inlineCode>.:~/.flowpipe/config</inlineCode>).  This allows you to manage your <a href="/docs/reference/config-files/workspace"> workspaces </a> and <a href="/docs/reference/config-files/credential/index">credentials</a> centrally in the <inlineCode>~/.flowpipe/config</inlineCode> directory, but override them in the working directory / mod location if desired.
    </td> 
  </tr>

  <tr> 
    <td nowrap="true"> <inlineCode>-h</inlineCode>, <inlineCode>--help</inlineCode> </td> 
    <td>  Help for Flowpipe. </td> 
  </tr>
                  
  <tr> 
    <td nowrap="true"> <inlineCode>--host</inlineCode> </td> 
    <td> Run the command against a local or remote server instance.  You may specify the full host and port (e.g. <inlineCode>--host https://flowpipe.my-org.com:7103</inlineCode>), or use the keyword <inlineCode>local</inlineCode> to connect to the local server instance as a shortcut for <inlineCode>https://localhost:7103</inlineCode> (e.g. <inlineCode>--host local</inlineCode>) </td> 
  </tr>

  <tr> 
    <td nowrap="true">  <inlineCode>--insecure</inlineCode> </td> 
    <td> Ignore any TLS certificate errors and warnings when connecting to a Flowpipe API host. </td> 
  </tr>



  <tr> 
    <td nowrap="true"> <inlineCode>--mod-location</inlineCode>  </td> 
    <td> Sets the Flowpipe workspace working directory.  If not specified, the workspace directory will be set to the current working directory.  See <a href="/docs/reference/env-vars/flowpipe_mod_location">FLOWPIPE_MOD_LOCATION</a> for details. </td>
  </tr>

   <tr> 
    <td nowrap="true">  <inlineCode>--output</inlineCode> </td> 
    <td>  Select a console output format: <inlineCode>pretty</inlineCode>, <inlineCode>plain</inlineCode>, <inlineCode>yaml</inlineCode> or <inlineCode>json</inlineCode> (default <inlineCode>pretty</inlineCode>). </td>
  </tr>

  <tr> 
    <td nowrap="true"> <inlineCode>-v</inlineCode>, <inlineCode>--version</inlineCode>  </td> 
    <td>  Display Flowpipe version. </td> 
  </tr>

  <tr> 
    <td nowrap="true"> <inlineCode>--workspace	</inlineCode>  </td> 
    <td>  Sets the Flowpipe workspace profile. If not specified, the default workspace will be used if it exists. See <a href="/docs/reference/env-vars/flowpipe_workspace">FLOWPIPE_WORKSPACE</a> for details. </td> 
  </tr>


  



</table>



<!--

  <tr> 
    <td nowrap="true"> <inlineCode>--cloud-host</inlineCode>  </td> 
    <td>  Sets the Flowpipe Cloud host used when connecting to Flowpipe Cloud workspaces.  See <a href="/docs/reference/env-vars/flowpipe_cloud_host">STEAMPIPE_CLOUD_HOST</a> for details. </td>
  </tr>

  <tr> 
    <td nowrap="true"> <inlineCode>--cloud-token</inlineCode>  </td> 
    <td>  Sets the Flowpipe Cloud authentication token used when connecting to Flowpipe Cloud workspaces.  See <a href="/docs/reference/env-vars/flowpipe_cloud_token">STEAMPIPE_CLOUD_TOKEN</a> for details. </td>
  </tr>



  <tr> 
    <td nowrap="true"> <inlineCode>--workspace</inlineCode>  </td> 
    <td>  Sets the Flowpipe <a href="/docs/run/workspaces"> workspace profile</a>.  If not specified, the <inlineCode>default</inlineCode> workspace will be used if it exists.  See <a href="/docs/reference/env-vars/flowpipe_workspace">STEAMPIPE_WORKSPACE</a> for details.</td>
  </tr>


  <tr> 
    <td nowrap="true"> <inlineCode>--workspace-database</inlineCode>  </td> 
    <td>  Sets the database that Flowpipe will connect to. This can be <inlineCode>local</inlineCode> (the default) or a remote Flowpipe Cloud database.  See <a href="/docs/reference/env-vars/flowpipe_workspace_database">STEAMPIPE_WORKSPACE_DATABASE</a> for details. </td>
  </tr>

-->


---

