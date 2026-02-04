# Apache Web Server (Podman)

A minimal Apache HTTP Server container built on Red Hat UBI 10.
Designed to run securely with **rootless Podman**.

## Features

- Red Hat UBI 10 base image
- Rootless-compatible (non-privileged port)
- Runs as non-root `apache` user
- Simple static content serving

## Build

From the `apache-webserver` directory:

```bash
podman build -t apache-webserver .
```
