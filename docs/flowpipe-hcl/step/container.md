---
title: container
sidebar_label: container
---


# container
A `container` step is used to run a Docker container.

> Important!  Flowpipe `container` steps require Docker to be running.  Make sure you have the Docker daemon installed and running.

Each container definition is a distinct instance of the step (a distinct container image). A looped step (via `for_each`) will create a container instance for each iteration.


```hcl
pipeline "another_pipe" {

  step "container" "aws_s3_ls" {
     image      = "public.ecr.aws/aws-cli/aws-cli"
     cmd        = ["s3", "ls"]

     env = {
      AWS_ACCESS_KEY_ID     = "AKIAFAKEKEY25WNAQKRC"
      AWS_SECRET_ACCESS_KEY = "+bOnLljsFEZfakekeyJWQq7kkQaOsh5JbkTQH5YR"
     }
  }  

  output "buckets" {
    value = step.container.aws_s3_ls.stdout
  }
}

```



## Arguments

| Argument        | Type      | Optional?   | Description
|-----------------|-----------|-------------|-----------------
| `image`         | String	  | Required    | The docker image to use, eg `turbot/steampipe:latest`.  
| `cmd`           | List of String| Optional  |	The cmd to use to start the container. 
| `cpu_shares`    | Number    | Optional    | CPU shares (relative weight) for the container.
| `entrypoint`    | List of String | Optional  | Overwrite the default `ENTRYPOINT` of the image. The entrypoint allows you to configure a container to run an executable. 
| `env`           | Map	of Strings | Optional	  | A map of string to set environment variables
| `memory`        | Number	  | Optional	  | Amount of memory in MB your container can use at runtime. Defaults to `128`.
| `memory_reservation` | Number | Optional	  | Allows you to specify a soft limit smaller than `memory` which is activated when Docker detects contention or low memory on the host machine. If you use `memory-reservation`, it must be set lower than `memory` for it to take precedence. Because it is a soft limit, it does not guarantee that the container doesn't exceed the limit.
| `memory_swap`   | Number	  | Optional	  | The amount of memory this container is allowed to swap to disk.  If `memory` and `memory-swap` are set to the same value, this prevents containers from using any swap. This is because `memory-swap` is the amount of combined memory and swap that can be used, while `memory` is only the amount of physical memory that can be used.  Set to `-1` to enable unlimited swap
| `memory_swappiness` | Number	  | Optional	  | Tune container memory swappiness (0 to 100).  
|`read_only`      | Boolean	  | Optional | If `true`, the container will be started as read-only. Defaults to `false`.
| `user`          | String  | Optional | User to run the container as (format: `<name\|uid>[:<group\|gid>]`)
| `workdir`       | String    | Optional | The working directory for commands to run in.

<!-- Pulled for 0.1.0

| `image`         | String	  | Optional*	  | The docker image to use, eg `turbot/steampipe:latest`.  You must specify `image` or `source` but not both.
| `source`        | String	  | Optional*	  | The path to a folder that contains the `dockerfile` or `containerfile` to build the container.  You must specify `image` or `source` but not both.

-->

This step also supports the [common step arguments](/docs/flowpipe-hcl/step/index#common-step-arguments) and [attributes](/docs/flowpipe-hcl/step/index#common-step-attributes-read-only).



## Attributes (Read-Only)

| Attribute       | Type    |  Description
|-----------------|---------|-----------------
| `container_id`  | String  | The container id
| `exit_code`     | Number  | The exit code from running the container
| `lines`         | List of objects | [Ordered list of all lines of output](#lines) (stdout & stderr) from the container
| `stdout`        | String  | `STDOUT` output from the container
| `stderr`        | String  | `STDERR` output from the container

### lines

The `lines` attribute provides a list of all lines of output from the container, in order.  `lines` is a list of objects, where each object has a `stream` (`stdout` or `stdout`) and a `line` (the line of output), for example:

```hcl
[
  {
    stream = "stdout"
    line = "This is the first"
  },
  {
    stream = "stdout"
    line = "block of STDOUT"
  },
  {
    stream = "stderr"
    line = "This is the first"
  },
  {
    stream = "stderr"
    line = "block of STDERR"
  },
  {
    stream = "stdout"
    line = "This is the second"
  },
  {
    stream = "stdout"
    line = "block of STDOUT"
  },      
]
```

Usage example:


```hcl

pipeline "simple_pipe" {

  step "container" "hello" {
     image = "hello-world"
  }

  output "combined_lines" {
    value = step.container.hello.lines[*].line
  }

  output "stdout_lines" {
    value = [for v in step.container.hello.lines[*] : v.line if v.stream == "stdout"]
  }

  output "stderr_lines" {
    value = [for v in step.container.hello.lines[*] : v.line if v.stream == "stderr"]
  }

  output "combined_text" {
    value = join("\n",step.container.hello.lines[*].line)
  }

  output "stdout_text" {
    value = join("\n",[for v in step.container.hello.lines[*] : v.line if v.stream == "stdout"])
  }

  output "stderr_text" {
    value = join("\n",[for v in step.container.hello.lines[*] : v.line if v.stream == "stderr"])
  }
}
```

<!--
## Source v/s Image
You must specify either an `image` or a `source` but not both.   The `image` will be pulled via standard docker client commands - The image must be publicly accessible or you must `docker login` to access a private repo.  You may instead specify a `source` in which case a custom image will be built before the step is run.  The image will be updated when anything in the `source` directory changes, and it will be deleted if the step is deleted. 
-->

## Labels
Docker labels are automatically added to images, containers, volumes, etc using the [standard format](https://docs.docker.com/config/labels-custom-metadata/), including the standard `org.opencontainers` labels as well as Flowpipe-specific labels prefixed with `io.flowpipe`.

```json
//container
"Labels": {
    "io.flowpipe.name": "test_suite_mod.pipeline.with_functions.function.hello_nodejs_step",
    "io.flowpipe.type": "container",
    "org.opencontainers.container.created": "2023-10-12T04:29:32Z",
    "org.opencontainers.image.created": "2023-10-12T04:29:29Z"
}
```
