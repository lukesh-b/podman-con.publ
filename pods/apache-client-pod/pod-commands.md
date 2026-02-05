# Pod Commands – Apache Client Pod

This document provides a quick reference for creating, running, testing, and
cleaning up a Podman pod with multiple containers.

---

## Create Pod

Create a pod named `myNewPod` and expose port `8080` on the host:

```bash
podman pod create --name myNewPod -p 8080:8080
podman pod ps // verify pod creation
```

## Run containers Inside the pod

### Apache WebServer Container

```bash
podman run -d --name webserverContainer --pod myNewPod registry.redhat.io/rhel10/httpd-24:latest
```

### Client Container

```bash
podman run -d --name clientContainer --pod myNewPod registry.redhat.io/ubi10:latest sleep infinity
podman ps --pod // verify containers are created in the pod
```

## Test from host machine

```bash
curl http://127.0.0.1:8080 // From host machine
```

### Test Inter-Container Comms

```bash
podman exec -it clientContainer /bin/bash // Enter the client container
curl localhost:8080 // check what returns, it should return the webserverContainer content
```








