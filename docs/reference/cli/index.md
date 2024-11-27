---
title: Powerpipe CLI
---

# Powerpipe CLI

## Sub-Commands

| Command | Description
|-|-
| [powerpipe help](reference/cli/help)      | Help about any command.
| [powerpipe login](reference/cli/login)    | Log in to Turbot Pipes.
| [powerpipe mod](reference/cli/mod)        | Powerpipe mod management.
| [powerpipe benchmark](reference/cli/benchmark) | List, view, and run Powerpipe benchmarks.
| [powerpipe control](reference/cli/control)| List and view Powerpipe controls.
| [powerpipe dashboard](reference/cli/dashboard) | List and view Powerpipe dashboards.
| [powerpipe query](reference/cli/query)    | List and view Powerpipe queries.
| [powerpipe server](reference/cli/server)  | Run Powerpipe server, including triggers and integrations.
| [powerpipe variable](reference/cli/variable)| List and view Powerpipe variables.


## Global Flags

<table>
  <tr> 
    <th> Flag </th> 
    <th> Description </th> 
  </tr>

  <tr> 
    <td nowrap="true"> <inlineCode>--config-path</inlineCode> </td> 
    <td>  
    Sets the search path for <a href = "/docs/reference/config-files">configuration files</a>. This argument accepts a colon-separated list of directories.  All configuration files (<inlineCode>*.ppc</inlineCode>) will be loaded from each path, with decreasing precedence.  The default is <inlineCode>.:$POWERPIPE_INSTALL_DIR/config</inlineCode> (<inlineCode>.:~/.powerpipe/config</inlineCode>).  This allows you to manage your <a href="/docs/reference/config-files/workspace"> workspaces </a> and credentials centrally in the <inlineCode>~/.powerpipe/config</inlineCode> directory, but override them in the mod location if desired.
    </td> 
  </tr>   

  <tr> 
    <td nowrap="true"> <inlineCode>-h</inlineCode>, <inlineCode>--help</inlineCode> </td> 
    <td>  Help for Powerpipe. </td> 
  </tr>

  <tr> 
    <td nowrap="true"> <inlineCode>--install-dir</inlineCode>  </td> 
    <td> Set the installation directory for Powerpipe. Internal Powerpipe files will be written to and read from this path. The default is <inlineCode>~/.powerpipe</inlineCode>. By default, the <a href="/docs/run#configuration-files">configuration search path </a> is also relative to this installation directory.  See <a href="/docs/reference/env-vars/powerpipe_install_dir">POWERPIPE_INSTALL_DIR</a> for details.
    </td>
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


---

## Exit Codes

|  Value  |   Name                                | Description
|---------|---------------------------------------|----------------------------------------
|   **0** | `ExitCodeSuccessful`                  | Powerpipe ran successfully, with no runtime errors, control errors, or alarms
|   **1** | `ExitCodeControlsAlarm`               | `powerpipe benchmark run` or `powerpipe control run` completed with no runtime or control errors, but there were one or more alarms
|   **2** | `ExitCodeControlsError`               | `powerpipe benchmark run` or `powerpipe control run` completed with no runtime errors,  but one or more control errors occurred
|  **21** | `ExitCodeSnapshotCreationFailed`      | Snapshot creation failed
|  **22** | `ExitCodeSnapshotUploadFailed`        | Snapshot upload failed
|  **31** | `ExitCodeServiceSetupFailure`         | Service setup failed
|  **32** | `ExitCodeServiceStartupFailure`       | Service start failed
|  **33** | `ExitCodeServiceStopFailure`          | Service stop failed
|  **41** | `ExitCodeQueryExecutionFailed`        | One or more queries failed for `powerpipe query run` 
|  **51** | `ExitCodeLoginCloudConnectionFailed`  | Connecting to Pipes failed
|  **61** | `ExitCodeModInitFailed`               | Mod init failed
|  **62** | `ExitCodeModInstallFailed`            | Mod install failed
| **249** | `ExitCodeInvalidExecutionEnvironment` | Powerpipe was run in an unsupported environment
| **250** | `ExitCodeInitializationFailed`        | Initialization failed
| **251** | `ExitCodeBindPortUnavailable`         | Network port binding failed
| **252** | `ExitCodeNoModFile`                   | The command requires a mod, but no mod file was found
| **253** | `ExitCodeFileSystemAccessFailure`     | File system access failed
| **254** | `ExitCodeInsufficientOrWrongInputs`   | Runtime error - insufficient or incorrect input
| **255** | `ExitCodeUnknownErrorPanic`           | Runtime error - an unknown panic occurred
