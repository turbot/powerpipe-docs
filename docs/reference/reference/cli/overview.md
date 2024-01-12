---
title: Powerpipe CLI
sidebar_label: Powerpipe CLI
---

# Powerpipe CLI

## Sub-Commands

| Command | Description
|-|-
| [powerpipe check](reference/cli/check)    | Run Powerpipe benchmarks and controls
| [powerpipe completion](reference/cli/completion)| Generate the autocompletion script for the specified shell
| [powerpipe dashboard](reference/cli/dashboard)| Powerpipe dashboards
| [powerpipe help](reference/cli/help)      | Help about any command
| [powerpipe login](reference/cli/login)        | Log in to Powerpipe CLoud
| [powerpipe mod](reference/cli/mod)        | Powerpipe mod management
| [powerpipe plugin](reference/cli/plugin)  | Powerpipe plugin management
| [powerpipe query](reference/cli/query)    | Execute SQL queries interactively or by argument
| [powerpipe service](reference/cli/service)| Powerpipe service management
| [powerpipe variable](reference/cli/variable)| Powerpipe variable management


## Global Flags


<table>
  <tr> 
    <th> Flag </th> 
    <th> Description </th> 
  </tr>

  <tr> 
    <td nowrap="true"> <inlineCode>-h</inlineCode>, <inlineCode>--help</inlineCode> </td> 
    <td>  Help for Powerpipe. </td> 
  </tr>
                  
  <tr> 
    <td nowrap="true"> <inlineCode>--install-dir</inlineCode>  </td> 
    <td>  Sets the directory for the Powerpipe installation, in which the Powerpipe database, plugins, and supporting files can be found.  See <a href="/docs/reference/env-vars/powerpipe_install_dir">STEAMPIPE_INSTALL_DIR</a> for details. </td>
  </tr>

  <tr> 
    <td nowrap="true"> <inlineCode>--workspace</inlineCode>  </td> 
    <td>  Sets the Powerpipe <a href="/docs/managing/workspaces"> workspace profile</a>.  If not specified, the <inlineCode>default</inlineCode> workspace will be used if it exists.  See <a href="/docs/reference/env-vars/powerpipe_workspace">STEAMPIPE_WORKSPACE</a> for details.</td>
  </tr>

<!--
  <tr> 
    <td nowrap="true"> <inlineCode>--schema-comments</inlineCode></td> 
    <td>   Include schema comments when importing connection schemas (default true).  Set to false to reduce the load time for very high connection counts.  If you disable schema comments, the inspect command will not have descriptions. </td> 
  </tr>

-->
  <tr> 
    <td nowrap="true"> <inlineCode>-v</inlineCode>, <inlineCode>--version</inlineCode>  </td> 
    <td>  Display Powerpipe version. </td> 
  </tr>

</table>

## Exit Codes

|  Value  |   Name                                | Description
|---------|---------------------------------------|----------------------------------------
|   **0** | `ExitCodeSuccessful`                  | Powerpipe ran successfully, with no runtime errors, control errors, or alarms
|   **1** | `ExitCodeControlsAlarm`               | `powerpipe check` completed with no runtime or control errors, but there were one or more alarms
|   **2** | `ExitCodeControlsError`               | `powerpipe check` completed with no runtime errors,  but one or more control errors occurred
|  **11** | `ExitCodePluginLoadingError`          | Plugin loading error
|  **12** | `ExitCodePluginListFailure`           | Plugin listing failed
|  **13** | `ExitCodePluginNotFound`              | Plugin not found
|  **14** | `ExitCodePluginInstallFailure`        | Plugin install failed
|  **21** | `ExitCodeSnapshotCreationFailed`      | Snapshot creation failed
|  **22** | `ExitCodeSnapshotUploadFailed`        | Snapshot upload failed
|  **31** | `ExitCodeServiceSetupFailure`         | Service setup failed
|  **32** | `ExitCodeServiceStartupFailure`       | Service start failed
|  **33** | `ExitCodeServiceStopFailure`          | Service stop failed
|  **41** | `ExitCodeQueryExecutionFailed`        | One or more queries failed for `powerpipe query` 
|  **51** | `ExitCodeLoginCloudConnectionFailed`  | Connecting to cloud failed
|  **61** | `ExitCodeModInitFailed`               | Mod init failed
|  **62** | `ExitCodeModInstallFailed`            | Mod install failed
| **249** | `ExitCodeInvalidExecutionEnvironment` | Powerpipe was run in an unsupported environment
| **250** | `ExitCodeInitializationFailed`        | Initialization failed
| **251** | `ExitCodeBindPortUnavailable`         | Network port binding failed
| **252** | `ExitCodeNoModFile`                   | The command requires a mod, but no mod file was found
| **253** | `ExitCodeFileSystemAccessFailure`     | File system access failed
| **254** | `ExitCodeInsufficientOrWrongInputs`   | Runtime error - insufficient or incorrect input
| **255** | `ExitCodeUnknownErrorPanic`           | Runtime error - an unknown panic occurred
