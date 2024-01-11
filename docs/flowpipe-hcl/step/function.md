---
title: function
sidebar_label: function
---

# function

A `function` step is used to run an AWS Lambda-compatible function.

> Important!  Flowpipe `function` steps require Docker to be running.  Make sure you have the Docker daemon installed and running.


Each step *definition* is a distinct instance of the step (a distinct container image); a looped step (via `foreach`) is ONE definition, but you cannot change the `runtime` or other container-specific settings in the loop. 

```hcl
pipeline "simple_pipe" {
  param "event" {}

  step "function" "my_func_defaults" {
      source    = "./my-other-function"  
      event     = param.event
  }
}
```

## Arguments

| Argument        | Type      | Optional?   | Description
|-----------------|-----------|-------------|-----------------
| `env`           | Map	      | Optional	  | A map of string to set environment variables
| `event`         | Object	  | Optional	  | JSON-compatible [event](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-concepts.html#gettingstarted-concepts-event) object that contains data for a Lambda function to process
| `source`        | String	  | Required	  | Path to the function's deployment package within the local filesystem. The path is relative to the root of the mod (where the `mod.hcl`).
| `handler`       | String	  | Optional	  | Function entry point in your code, in the format `filename.functionname`.  If not specified default handler for the runtime is used (`index.handler` for nodejs, `lambda_function.lambda_handler` for python, etc.)
| `runtime`       | String	  | Optional	  | Identifier of the function's runtime. Valid Values are `nodejs:18`, `python:3.10`.

This step also supports the [common step arguments](/docs/flowpipe-hcl/step/index#common-step-arguments) and [attributes](/docs/flowpipe-hcl/step/index#common-step-attributes-read-only).

<!--
| `memory`        | Number	  | Optional	  | Amount of memory in MB your Lambda Function can use at runtime. Defaults to 128.
| `max_parallel`  | Number	  | Optional	  | Maximum number of concurrent executions for this function. A value of -1 removes any concurrency limitations. Defaults to `-1`.

-->

## Attributes (Read-Only)

| Attribute       | Type    |  Description
|-----------------|---------|-----------------
| `response`      | Object  | The response returned by the function

<!--
| `exit_code`     | Number  | The exit code from running the container
-->




## Names & Labels

Container names are randomly generated using standard docker names (`elegant_sutherland`, etc).

Image names are generated using the following format: `flowpipe/{mod}.pipeline.{pipeline name}.function.{step name}:{timestamp}`, e.g: `flowpipe/test_suite_mod.pipeline.with_functions.function.hello_nodejs_step:20231012T155317200`

Docker labels are automatically added to images & containers using the [standard format](https://docs.docker.com/config/labels-custom-metadata/), including the standard `org.opencontainers` labels, as well as Flowpipe-specific labels prefixed with `io.flowpipe`.

```json
//container
"Labels": {
    "io.flowpipe.name": "test_suite_mod.pipeline.with_functions.function.hello_nodejs_step",
    "io.flowpipe.runtime": "nodejs:18",
    "io.flowpipe.type": "function",
    "org.opencontainers.container.created": "2023-10-12T04:29:32Z",
    "org.opencontainers.image.created": "2023-10-12T04:29:29Z"
}
```


## More Examples


```hcl
pipeline "another_pipe" {

  step "function" "my_func" {
    # Now
    description  = "this is my function" 
    source       = "./my-function"  
    runtime      = "nodejs:18" 
    handler      = "my_file.my_handler"
    timeout      = 10000 

    event = jsondecode(jsonencode({
      "Records":[
        {
          "messageId":"916f5e95-bbbb-4148-1234-2ac8e764f06c",
          "receiptHandle":"AQEBmLuoGWtLtFFgvyCFdSPMJh2HKgHOIPWNUq22EOwCzGT8iILZm97CE6j4J6oR71ZpDr3sgxQcJyVZ+dmmvGl+fFftT9GCJqZYrjMGsR2Q6WsMd8ciI8bTtDXyvsk8ektd7UGfh4gxIZoFp7WUKVRcMEeBkubKd8T4/Io81D0l/AK7MxcEfCj40vWEsex1kkGmMRlBtdSeGyy7fJgUq2543AYWciiWtbSit8S0Y38xZPmsIFhoxP0egQRoJcW4aUgMi469Gj5+khizetybtgC8vux5NCg/IejxcCueXkQ7LKVF8kfRdqRSUYB6DsOrfakeZpK4wpXIarByNz0R2p7J88meYpj2IVULv/emXsSYaKG4rXnpbH4J9ijbLWckYLAd7wPDzCYri1ZSTgAz0kchsEw==",
          "body":"{\n\"name\": \"bob\",\n\"tag\": \"hello\"\n}",
          "attributes":{
              "ApproximateReceiveCount":"1",
              "SentTimestamp":"1602046897707",
              "SenderId":"AIDAR3FAKE4FCWXL56NUU",
              "ApproximateFirstReceiveTimestamp":"1602046897712"
          },
          "messageAttributes":{
              
          },
          "md5OfBody":"98da683a47692b39c1d43bd4fa21ed89",
          "eventSource":"aws:sqs",
          "eventSourceARN":"arn:aws:sqs:ap-south-1:123456789012:documentation",
          "awsRegion":"ap-south-1"
        }
      ]
    }))

    env = {
      foo = "bar"
      up  = "down"
    }
  }
}

```