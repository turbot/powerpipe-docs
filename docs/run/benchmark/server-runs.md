---
title: Run Benchmarks on a Server
---

# Run Benchmarks on a Server

`powerpipe benchmark run` loads the mod each time you run it. If you run many benchmarks, for example from a scheduler, you can load the mod one time in a [Powerpipe server](/docs/run/server) and run each benchmark on that server. The runs share the loaded mod and the database connections of the server, so they use less memory and start faster.

```
powerpipe benchmark run cis_v300 --host http://localhost:9033
        │
        │  start the run, read its state, download the snapshot
        ▼
powerpipe server  (mod loaded one time)  ──▶  database
```

You can use the server in two ways:

- the `--host` argument of `powerpipe benchmark run`;
- the HTTP API of the server.

## Run a benchmark with `--host`

Start the server in the mod directory:

```bash
powerpipe server
```

In a different terminal, run a benchmark on the server and export a snapshot:

```bash
powerpipe benchmark run cis_v300 --host http://localhost:9033 --output none --export cis_v300.pps
```

The command starts the run on the server, waits for it, writes the snapshot, and removes the run from the server. It exits with the same [exit code](/docs/reference/cli#exit-codes) as a local run: `0`, `1` (alarms) or `2` (control errors).

The server loads the mod and runs the controls, so with `--host` you can use only these arguments:

| Argument | Description
|-|-
| `--output` | `snapshot` (or `pps`) or `none`.
| `--export` | A snapshot file, for example `--export cis_v300.pps`, or `--export pps` for the default file name. Other export formats are not supported.
| `--benchmark-timeout` | The timeout for the run, in seconds.
| `--progress` | Show or hide the progress information.

You can run one benchmark at a time. Arguments that change the mod, the database or the controls, for example `--mod-location`, `--database`, `--var`, `--where`, `--tag`, `--max-parallel` and `--share`, give an error with `--host`. So do `--var-file` and a `PP_VAR_<name>` environment variable: the run always uses the variable values that the server loaded. To change these, change the server.

If the server is reloading the mod when the command starts the run, the command waits and tries again until the reload is complete. See [When the server reloads the mod](#when-the-server-reloads-the-mod).

If you stop the command with Ctrl-C while the run is running, Powerpipe cancels and removes the run on the server, and exits with code `91`.

The server must run Powerpipe v1.6.0 or later. An older server gives an error that it cannot run benchmarks.

## Run a benchmark with the HTTP API

The API has no authentication: anyone who can reach the server can start, read and remove runs. Keep the server on `--listen local` (the default), or put it behind a proxy that authenticates requests.

The server has these endpoints:

| Request | Response
|-|-
| `POST /api/v0/benchmark/{benchmark}/run` | `202`: the run starts. The `Location` header is the URL of the run. `400` if the request does not have `Content-Type: application/json`, or the body is not valid. `404` if `{benchmark}` is not a benchmark. `413` if the body is larger than 64 KiB. `429` if 10 runs are already running: try again when one has finished. `503` if the server is reloading the mod: try again after the number of seconds in the `Retry-After` header.
| `GET /api/v0/benchmark/{benchmark}/run/{run_id}` | `200`: the state of the run. `404` if there is no such run.
| `GET /api/v0/benchmark/{benchmark}/run/{run_id}/snapshot` | `200`: the snapshot of the run, the same as `--export <file>.pps`. `404` if there is no such run. `409` if the run is still running.
| `DELETE /api/v0/benchmark/{benchmark}/run/{run_id}` | `204`: the run is removed. If the run is running, it is cancelled. `404` if there is no such run.
| `GET /api/v0/workspace` | `200`: the [load state](#the-load-state) of the mod.

`{benchmark}` is the name of the benchmark, in any form that `powerpipe benchmark run` accepts: `cis_v300`, `benchmark.cis_v300`, `aws_compliance.benchmark.cis_v300` or `all`. The URL of the run uses the full name, for example `aws_compliance.benchmark.cis_v300`.

Errors other than `413` have a JSON body that describes the error.

### Example

Start a run. The request must have the header `Content-Type: application/json`. The body is optional:

```bash
curl -i -X POST \
  -H 'Content-Type: application/json' \
  -d '{"timeout_seconds": 600}' \
  http://localhost:9033/api/v0/benchmark/cis_v300/run
```

```
HTTP/1.1 202 Accepted
Location: /api/v0/benchmark/aws_compliance.benchmark.cis_v300/run/0f4e0c9a5d3b4b7e

{
  "id": "0f4e0c9a5d3b4b7e",
  "benchmark": "aws_compliance.benchmark.cis_v300",
  "status": "running",
  "start_time": "2026-11-10T10:00:00Z"
}
```

Read the state of the run until its `status` is not `running`:

```bash
curl http://localhost:9033/api/v0/benchmark/aws_compliance.benchmark.cis_v300/run/0f4e0c9a5d3b4b7e
```

```json
{
  "id": "0f4e0c9a5d3b4b7e",
  "benchmark": "aws_compliance.benchmark.cis_v300",
  "status": "complete",
  "start_time": "2026-11-10T10:00:00Z",
  "end_time": "2026-11-10T10:02:13Z",
  "summary": { "alarm": 12, "ok": 230, "info": 4, "skip": 0, "error": 0 }
}
```

Download the snapshot, then remove the run:

```bash
curl -o cis_v300.pps http://localhost:9033/api/v0/benchmark/aws_compliance.benchmark.cis_v300/run/0f4e0c9a5d3b4b7e/snapshot
curl -X DELETE http://localhost:9033/api/v0/benchmark/aws_compliance.benchmark.cis_v300/run/0f4e0c9a5d3b4b7e
```

### The run

| Field | Description
|-|-
| `id` | The ID of the run.
| `benchmark` | The full name of the benchmark.
| `status` | `running`, `complete` or `error`.
| `error` | The error, if the status is `error`.
| `start_time` | The time the run started.
| `end_time` | The time the run stopped. Not set while the run is running.
| `summary` | The control status counts, when the run is not running.

`timeout_seconds` works as `--benchmark-timeout`. A run that times out has the status `error`, and you can still download its snapshot.

### When the server removes a run

The server keeps the result rows of a run on disk until the run is removed. Remove each run with `DELETE` when you have its snapshot.

If no client reads a run for one minute, the server cancels it (if it is running) and removes it. A snapshot download counts as a read. This removes the runs of a client that stops before it can delete them. `powerpipe benchmark run --host` reads its run every second.

## When the server reloads the mod

The server reloads the mod when:

- a mod or variable file in the mod directory changes, for example `mod.pp` or `powerpipe.ppvars`;
- you install, update or uninstall mods in the mod directory, for example with `powerpipe mod update`.

A large mod can take some minutes to reload. While the server reloads the mod:

- a request to start a run gets `503`, with a `Retry-After` header that gives the number of seconds to wait;
- runs that are running continue, and their URLs (with the full name of the benchmark) continue to answer;
- `powerpipe benchmark run --host` waits and tries again.

If the reload fails, for example because a file has an error, the server keeps the mod as it was before the reload.

The `503` answer and the load state need a server that runs Powerpipe v1.7.0 or later.

### The load state

`GET /api/v0/workspace` gives the load state of the mod. It always answers at once, also while the server reloads the mod:

```json
{
  "loading": false,
  "load_count": 3,
  "last_load": {
    "start_time": "2026-11-21T10:00:00.120Z",
    "end_time": "2026-11-21T10:01:52.480Z",
    "newest_file_time": "2026-11-21T09:59:58.904127311Z"
  }
}
```

| Field | Description
|-|-
| `loading` | `true` while the server loads the mod.
| `load_count` | The number of loads that are complete, including the first load and loads that failed.
| `current_load` | The load that is in progress, if there is one.
| `last_load` | The last load that is complete.
| `start_time`, `end_time` | The times the load started and stopped.
| `newest_file_time` | The newest change time of the mod files, the variable files, the lock file and their directories when the load started. The load read all changes made up to this time.
| `error` | The error, if the load failed.

The server answers only after its first load is complete. If the first load fails, the server stops.

### Wait for a change to load

If you change the files of the mod and then run a benchmark, wait for the server to load the change first. Otherwise the run can use the mod as it was before the change.

1. After your last change, get the change time (`ctime`) of each file that you changed: `stat -c %z <file>` on Linux, `stat -f %Fc <file>` on macOS. For a file that you deleted, use the change time of its directory. After `powerpipe mod install`, `mod update` or `mod uninstall`, use the lock file `.mod.cache.json`.
2. Read `GET /api/v0/workspace` until `loading` is `false` and `last_load.newest_file_time` is the same as or after the newest of those times.
3. If that load has an `error`, the change did not load. Correct the files and try again.

A copy that keeps the modification time of the original file, for example `cp -p`, `rsync -t` or `tar`, still sets a new change time.
