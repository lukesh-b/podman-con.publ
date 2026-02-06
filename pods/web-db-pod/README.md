# Web and Database Application Using Podman Pod

This project demonstrates how to run a **multi-container application** using a **Podman pod**:

- A **MariaDB database container** with persistent storage using a Podman volume
- An **Apache httpd web container** using a bind mount for web content

Both containers run inside a single Podman pod and share the same network namespace.

---

## Architecture Overview

| Component | Technology | Storage Type |
|---------|-----------|--------------|
| Database | MariaDB | Podman Volume |
| Web Server | Apache httpd | Bind Mount |
| Runtime | Podman Pod | Shared Network |


## Architecture Diagram

```mermaid
graph TD
    subgraph web-db-pod [Pod: web-db-pod]
        httpd[HTTPD Container<br>/var/www/html (Bind Mount)]
        mariadb[MariaDB Container<br>/var/lib/mysql (Volume)]
    end

    httpd -->|Connects to| mariadb
```

---

## Prerequisites

- Podman installed (v4 or later recommended)
- Access to `registry.redhat.io`
- SELinux enabled (examples use `:Z`)
- Working directory set to `web-db-pod`

---

## Directory Structure
```markdown
web-db-pod/
├── README.md
└── html/
└── index.html
```

---

## Step 1: Create a pod that exposes port **8080** to the host.
```bash
podman pod create --name web-db-pod -p 8080:8080
podman pod ls // verify pod created properly
```

## Step 2: Create Persistent Volume for MariaDB
```bash
podman volume create mariadb-data-vol
podman volume ls // verify volume created properly
```

## Step 3: Run MariaDB Container (Volume-backed)
```bash
podman run -d --name mariadb-container --pod web-db-pod -v mariadb-data-vol:/var/lib/mysql:Z -e MYSQL_USER=appuser -e MYSQL_PASSWORD=apppassword -e MYSQL_DATABASE=appdb -e MYSQL_ROOT_PASSWORD=rootpassword registry.redhat.io/rhel9/mariadb-105:latest
podman ps // verify container created and running
```

## Step 4: Prepare Web Content on Host (Bind Mount)
```bash
mkdir -p html
echo "Hello World" > html/index.html
```

## Step 5: Run httpd Container (Bind Mount-backed)
```bash
podman run -d --name httpd-container --pod web-db-pod -v $(pwd)/html:/var/www/html:Z registry.redhat.io/rhel10/httpd-24:latest
podman ps //verify container is created and running
```

## Step 6: Access the Web Application
```bash
curl localhost:8080 // should display Hello World
```

## Step 7: Validate Bind Mount Behavior
```bash
echo "Hello World - Updated via Bind Mount" > html/index.html
curl localhost:8080 //should display new content
```

## Cleanup
```bash
podman pod rm -f web-db-pod
podman volume rm mariadb-data-vol
```







