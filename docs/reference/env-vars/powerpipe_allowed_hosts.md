---
title: POWERPIPE_ALLOWED_HOSTS
---

# POWERPIPE_ALLOWED_HOSTS
Sets the host names of `powerpipe server` besides `localhost`, as a comma-separated list. A name that starts with a dot, such as `.example.com`, also matches its subdomains. This value can only be set when the server is started. It is the same as the `--allowed-hosts` argument of [powerpipe server](/docs/reference/cli/server).

You do not need it to open the dashboards at `http://localhost:9033`. Set it when you reach the server by another name, for example through a reverse proxy:

- With `--listen local` (the default), the server accepts a request only when its `Host` header is `localhost`, a loopback IP address, or one of these hosts. This stops a web page from reaching your server through DNS rebinding.
- The server accepts a WebSocket connection only from a page on its own origin (the same host and port) or on one of these hosts. This stops other web pages that you visit from connecting to your server and reading your dashboards.

## Usage

Serve the dashboards through a proxy at `https://powerpipe.example.com`:

```bash
export POWERPIPE_ALLOWED_HOSTS=powerpipe.example.com
```

Allow a domain and all of its subdomains:

```bash
export POWERPIPE_ALLOWED_HOSTS=.example.com
```

Develop the dashboard UI, where the development server on `http://localhost:3000` connects to `powerpipe server` on `http://localhost:9033`:

```bash
export POWERPIPE_ALLOWED_HOSTS=localhost
```
