---
title: http
sidebar_label: http
---

# http

An `http` trigger is used to create a webhook and initiate a pipeline when a request is made to the webhook.

```hcl
trigger "http" "my_webhook" {
  pipeline = pipeline.my_pipeline
  args     = {
    event = self.request_body
  }                              
}
```

It is common to pass in the `request_body` and/or `request_header` trigger attributes as arguments to the pipeline using `self` to reference the trigger instance.  


##### Arguments


| Argument        | Type    | Optional?  | Description
|-----------------|---------|------------|-----------------
| `args`	        | Map	    | Optional	 | A map of arguments to pass to the pipeline.
| `description`   |  String | Optional   | A string containing a short description of the step. 
| `documentation` | String (Markdown)| Optional | A markdown string containing a long form description, used as documentation for the mod on hub.flowpipe.io. 
| `execution_mode` | String  | Optional   | Specifies whether the trigger should wait for the pipeline to complete and return its results in the response body (`synchronous`), or return immediately.  The default is `asynchronous`,
| `pipeline`      | Pipeline Reference | Optional | A reference to a `pipeline` resource to start when this trigger runs.  
| `tags` | Map | Optional | A map of key:value metadata for the mod, used to categorize, search, and filter.   
| `title` | String | Optional | Display title for the step.


<!--
| `method`        | [method](#http-methods) block   | Optional | A supported HTTP [http-methods](#method) block.  The block label must be one of `get`, `post`.  Note that if no `post` method block appears, only `post` is supported, and the top-level `pipeline`, `args`, and `execution_mode` apply. 

-->

##### Attributes (Read-Only)

| Attribute       | Type    |  Description
|-----------------|---------|-----------------
| `request_body`  | String  | The request body as a string.
| `request_headers` | Map   | A map of request header field names and values.
| `status_code`   | Number  | The HTTP response status code.
| `url`	          | String	| The webhook URL.


<!--

## HTTP Methods

By default, the http trigger will only fire on `POST` events. You can use `method` blocks to trigger different pipelines for different HTTP methods.  The block label must be the method name (`get` or `post`).

Note that if no method blocks appear, only `post` is supported, and the top-level `pipeline`, `args`, and `execution_mode` apply.


```hcl
trigger "http" "my_webhook" {

  method "post" {
    pipeline = pipeline.my_pipeline

    args = {
      event = self.request_body
    }
  }
    
  method "get" {
    execution_mode = "synchronous" 
    pipeline       = pipeline.confirm_setup

    args = {
      headers = self.request_headers
    }
  }
                              
}
```

| Argument        | Type    | Optional?  | Description
|-----------------|---------|------------|-----------------
| `args`	        | Map	    | Optional	 | A map of arguments to pass to the pipeline.
| `execution_mode`| String  | Optional   | Specifies whether the trigger should wait for the pipeline to complete and return its results in the response body (`synchronous`), or return immediately (`asynchronous`).  The default is `asynchronous`,
| `pipeline`      | Pipeline Reference | Optional | A reference to a `pipeline` resource to start when this trigger runs.  

-->


## Webhook Endpoint URL

Flowpipe creates an endpoint on the flowpipe server for each `http` trigger. The HTTP webhook does not support any authentication mechanism, but it does have a URL with randomness to make it unguessable.  The webhook URL path is: `/api/latest/hook/{trigger HCL label}/{random string}`, eg `/api/latest/hook/my_webhook/21ifp8truzi8y2r29jdl0qi7qt`

The webhook URL will remain consistent across restarts. The {random string} is generated using the trigger name (the block label of the `trigger` ) and a global salt value.  
  - Because the URL contains the trigger name, changing the trigger name will generate a new URL.  
  - The salt value is stored in `~/.flowpipe/internal/salt`
  - If the file is missing or empty, flowpipe will randomly generate a new salt and write it there.  If you want to change *all* of your webhook URLs, remove the salt value.
  - The file may contain more than one salt. If more than one value is specified, a URL will be generated for EACH salt value for every webhook.  The last one will be considered the "default" - the one that will be shown in `flowpipe trigger list` and available in the `url` HCL attribute for the trigger (`trigger.http.my_webhook.url`).  `flowpipe trigger show` will show all the trigger URLs.  
  

## Webhook Response  

Flowpipe will return metadata in the response headers to identify the trigger process execution ID and the pipeline process execution ID:
```bash
Flowpipe-Execution-Id:          exec_cl92cgjjtoj4uv7etkq0
Flowpipe-Pipeline-Execution-Id: pexec_cl92cgjjtoj4uv7etkqg  
```


If the HTTP trigger is configured to run a pipeline synchronously, it will return the outputs of the pipeline as top-level fields in the response. The response body will be formatted as JSON.  
  ```json
    {
      "my_string_outout": "foo",
      "my_list_output": [
        "one",
        "two",
        "three"
      ],
      "my_object_output": {
        "color": "blue",
        "number": 34
      }
    }
  ```
    



