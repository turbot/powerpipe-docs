---
title: STEAMPIPE_OTEL_LEVEL
sidebar_label: STEAMPIPE_OTEL_LEVEL
---
# STEAMPIPE_OTEL_LEVEL
Specify which [OpenTelemetry](https://opentelemetry.io/) data to send via OTLP.  Accepted values are:

| Level | Description
|-|-
| `ALL`     | Send Metrics and Traces via OTLP
| `METRICS` | Send Metrics via OTLP
| `NONE`    | Do not send OpenTelemetry data (default)
| `TRACE`   | Send Traces via OTLP

Powerpipe is instrumented with the Open Telemetry SDK which supports the [standard SDK environment variables](https://github.com/open-telemetry/opentelemetry-specification/blob/main/specification/sdk-environment-variables.md). If `OTEL_EXPORTER_OTLP_ENDPOINT` is not specified, Powerpipe will default to `localhost:4317`. 

Currently, Powerpipe only supports OTLP/gRPC. 

The Powerpipe Plugin SDK added OpenTelemetry support in version 3.3.0 - plugins must be compiled with `v3.3.0` or later.  



## Usage 

Trace a single query and send to default endpoint (`localhost:4317`):

```bash
STEAMPIPE_OTEL_LEVEL=TRACE powerpipe query "select * from aws_iam_role"
```

Turn on Metrics and Tracing for the session:
```bash
export OTEL_EXPORTER_OTLP_ENDPOINT=my-otel-collector.mydomain.com:4317 
export STEAMPIPE_OTEL_LEVEL=ALL 
powerpipe query
```




