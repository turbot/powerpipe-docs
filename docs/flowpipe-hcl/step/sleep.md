---
title: sleep
sidebar_label: sleep
---


# sleep

A `sleep` step can be used to wait for a defined amount of time.  You will usually want to add an explicit dependency when using a `sleep` step.

```hcl
step "sleep" "sleep_10_seconds" {
  depends_on = [ step.http.http_1 ]
  duration   = "10s"
}
```


## Arguments


| Argument        | Type    | Optional?  | Description
|-----------------|---------|------------|-----------------
| `duration`      | String or Number | Required | The amount of time to sleep as an integer or a [duration string](#duration-strings).  If the duration is an integer, it is interpreted as the number of milliseconds.  


This step also supports the [common step arguments](/docs/flowpipe-hcl/step/index#common-step-arguments) and [attributes](/docs/flowpipe-hcl/step/index#common-step-attributes-read-only).


## Duration Strings

The `duration` argument may be an integer or a [Go duration string](https://pkg.go.dev/time#Duration).  If the duration is an integer, it will be interpreted as the number of milliseconds:


```hcl
 step "sleep" "sleep_100_milliseconds" {
  duration = 100 
}
```

You may instead pass a string that specifies the number and type of units.  Valid time units are `ns`, `us` (or `µs`), `ms`, `s`, `m`, `h`:

```hcl
 step "sleep" "sleep_10_seconds" {
  duration = "10s"
}
```

You can even include multiple units:

```hcl
 step "sleep" "sleep_2_hours_and_45_minutes" {
  duration = "2h45m"
}
```