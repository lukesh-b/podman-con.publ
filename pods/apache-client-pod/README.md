# Apache Client Pod (Podman)

This example demonstrates a Podman pod running multiple containers that share the same network namespace.

## Pod Composition

The pod contains the following containers:

| Container Name       | Image                                      | Purpose |
|----------------------|--------------------------------------------|---------|
| infra (auto-created) | podman infra image                          | Pod networking |
| webserverContainer   | registry.redhat.io/rhel10/httpd-24:latest  | Apache HTTP server |
| clientContainer      | registry.redhat.io/ubi10:latest             | Client for testing connectivity |

---

## Architecture
```markdown
Host (port 8080)
        |
        v
+----------------------+
|   Pod: myNewPod      |
|                      |
|  localhost:8080      |
|   ├── Apache HTTPD   |
|   └── Client (curl)  |
+----------------------+
```
Note: All containers inside the pod share the same network namespace, allowing inter-container communication via `localhost` without exposing additional ports.
```
