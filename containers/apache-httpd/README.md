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

## Run

```bash
podman run -it --rm -p 8080:8080 apache-webserver
```

### Access the application

```yaml
http://localhost:8080
```

## Files

```yaml
.
├── Dockerfile
├── README.md
├── index.html
└── .gitignore
```

### Note

```yaml
-  Apache listens on port 8080 inside the container
-  Suitable for rootless podman environment
```

