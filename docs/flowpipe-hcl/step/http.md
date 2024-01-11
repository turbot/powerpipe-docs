---
title: http
sidebar_label: http
---

# http

An `http` step is used to make an HTTP request.


```hcl
pipeline "get_astronauts" {

  step "http" "whos_in_space" {
      url    = "http://api.open-notify.org/astros"
      method = "get"
  }

  output "people_in_space" {
    value = step.http.whos_in_space.response_body.people[*].name
  }
}
```


## Arguments

| Argument        | Type    | Optional?  | Description
|-----------------|---------|------------|-----------------
| `url`	          | String	| Required	  | The URL for the request. Supported schemes are `http` and `https`
| `basic_auth`    | Block   | Optional   | A `basic_auth` block to specify a username and password using [Basic Authentication](#basic-authentication)
| `ca_cert_pem`   | String  | Optional   | Certificate data of the Certificate Authority (CA) in PEM (RFC 1421) format.
| `insecure`      | Boolean | Optional   | Disables verification of the server's certificate chain and hostname. Defaults to `false`
| `method`        | Boolean | Optional   | The HTTP Method for the request. Allowed methods are a subset of methods defined in RFC7231 namely, `get`,`head`, and `post`, `put`, `patch`, `delete`. The default is `post`.
| `request_body`  | String  | Optional   | The request body as a string.
| `request_headers` | Map   | Optional   | A map of request header field names and values.


This step also supports the [common step arguments](/docs/flowpipe-hcl/step/index#common-step-arguments) and [attributes](/docs/flowpipe-hcl/step/index#common-step-attributes-read-only).

## Attributes (Read-Only)

| Attribute       | Type    |  Description
|-----------------|---------|-----------------
| `response_body` | String or object  | The response body. If the `Content-Type` is `application/json` then Flowpipe will decode it and return it as an object, otherwise, it is returned as a string.
| `response_headers` | Map of String | A map of response header field names and values. Duplicate headers are concatenated according to RFC2616.
| `status_code`   | Number  | The HTTP response status code.


## Basic Authentication
To use basic authentication, include a `basic_auth` block specifying a `username` and `password`.  This will create the appropriate `Authorization` header and add it to the request.

```hcl
step "http" "list_issues" {
  method = "get"
  url    = "${param.api_base_url}/rest/api/2/search?jql=project=${param.project_key}"
  request_headers = {
    Content-Type = "application/json"
  }

  basic_auth {
    username = param.user_email
    password = param.token
  }

}
```

| Attribute  | Type    |  Description
|------------|---------|-----------------
| `username` | String  | The basic auth username
| `password` | String  | The basic auth password

