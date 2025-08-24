# Compose vs CLI

## General Philosophy

- **Docker CLI (`docker run`, `docker network create`, etc.)**:
  You explicitly write out each command, container, volume, network, and environment variable every time.

- **Docker Compose (`docker-compose.yaml`)**:
  You declare your setup once in YAML, then let Compose orchestrate containers, networks, and volumes automatically.

In short:

- CLI = imperative (do this, then this, then this).
- Compose = declarative (this is my desired state, make it happen).

## Example Project

Web + database stack:

- Nginx (reverse proxy)
- Flask app (Python)
- PostgreSQL (database)

### Compose File

```yaml
version: "3.9"

services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - app

  app:
    build: ./app
    environment:
      DATABASE_URL: postgres://user:pass@db:5432/mydb
    volumes:
      - ./app:/usr/src/app
    depends_on:
      - db

  db:
    image: postgres:14
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: mydb
    volumes:
      - db_data:/var/lib/postgresql/data

volumes:
  db_data:
```

### CLI Equivalent

1. Create a network (default: project-scoped)

    ```bash
    docker network create myproject_default
    ```

2. Start DB

    ```bash
    docker run -d \
    --name myproject_db_1 \
    --network myproject_default \
    -e POSTGRES_USER=user \
    -e POSTGRES_PASSWORD=pass \
    -e POSTGRES_DB=mydb \
    -v myproject_db_data:/var/lib/postgresql/data \
    postgres:14
    ```

3. Start App

    ```bash
    docker build -t myproject_app ./app

    docker run -d \
    --name myproject_app_1 \
    --network myproject_default \
    -e DATABASE_URL=postgres://user:pass@db:5432/mydb \
    -v $(pwd)/app:/usr/src/app \
    myproject_app
    ```

4. Start Web

    ```bash
    docker run -d \
    --name myproject_web_1 \
    --network myproject_default \
    -p 8080:80 \
    -v $(pwd)/nginx.conf:/etc/nginx/nginx.conf:ro \
    nginx:alpine
    ```

## Breakdown of Key Features

| **Compose Directive**  | **What Compose Does**                                | **CLI Equivalent**                             |
| ---------------------- | ---------------------------------------------------- | ---------------------------------------------- |
| `services:`            | Defines named containers                             | `docker run ...` per service                   |
| `image:`               | Pulls or builds image                                | `docker pull` or `docker build`                |
| `build:`               | Builds image from context                            | `docker build -t <name> <path>`                |
| `ports:`               | Maps host\:container ports                           | `-p host:container`                            |
| `volumes:`             | Mounts host paths or named volumes                   | `-v host:container` or `docker volume create`  |
| `environment:`         | Injects env variables                                | `-e VAR=value`                                 |
| `depends_on:`          | Defines startup order (not health checks by default) | No direct CLI equivalent; must script manually |
| `networks:`            | Creates isolated networks per project                | `docker network create ...`                    |
| `volumes:` (top-level) | Declares named volumes                               | `docker volume create ...`                     |

## Operational Differences

Startup & teardown:

- Compose: `docker compose up -d` / `docker compose down`
- CLI: many `docker run`, `docker stop`, `docker rm`, `docker network rm`, etc.

Scaling:

- Compose: `docker compose up --scale app=3`
- CLI: must manually `docker run` multiple containers and connect them to the right network.

Logs:

- Compose: `docker compose logs -f`
- CLI: `docker logs -f <container>` (one at a time).

Configuration as code:

- Compose: everything is version-controlled in YAML.
- CLI: you’d need Bash scripts or Makefiles to replicate the stack.

## Mental Model

Think of Docker Compose as a **macro layer** over the Docker CLI:

- It automatically **names**, **creates networks**, **manages volumes**, and **organizes containers** under a project namespace.
- Every `docker-compose.yaml` can be **translated 1:1 into Docker CLI commands** — but Compose saves you from remembering all the flags and running them in the right order.

## Real life, big Compose example

### Step 0: Project Setup

We’ll assume the project is named `myproject`. Compose automatically prefixes containers, networks, and volumes with the project name unless overridden.

```bash
export PROJECT=myproject
```

### Step 1: Create Networks

Compose automatically creates networks listed under `networks:`.

```yaml
networks:
  webnet:
  backendnet:
```

**CLI Equivalent:**

```bash
docker network create ${PROJECT}_webnet
docker network create ${PROJECT}_backendnet
```

> **Caveat:** Compose networks are isolated by default. Containers on `backendnet` cannot see containers on `webnet` unless they are connected to both.

### Step 2: Create Named Volumes

Compose automatically creates volumes listed under `volumes:`.

```yaml
volumes:
  db_data:
  api_data:
  certs:
  prometheus_data:
  grafana_data:
```

**CLI Equivalent:**

```bash
docker volume create ${PROJECT}_db_data
docker volume create ${PROJECT}_api_data
docker volume create ${PROJECT}_certs
docker volume create ${PROJECT}_prometheus_data
docker volume create ${PROJECT}_grafana_data
```

> **Caveat:** Named volumes persist data across container recreation. Host bind mounts (like `./nginx.conf`) are **not auto-created**; you must have them locally.

### Step 3: Start the DB

```yaml
db:
  image: postgres:14
  environment:
    POSTGRES_USER: user
    POSTGRES_PASSWORD: pass
    POSTGRES_DB: mydb
  volumes:
    - db_data:/var/lib/postgresql/data
  networks:
    - backendnet
  restart: unless-stopped
```

**CLI Equivalent:**

```bash
docker run -d \
  --name ${PROJECT}_db_1 \
  --network ${PROJECT}_backendnet \
  -e POSTGRES_USER=user \
  -e POSTGRES_PASSWORD=pass \
  -e POSTGRES_DB=mydb \
  -v ${PROJECT}_db_data:/var/lib/postgresql/data \
  --restart unless-stopped \
  postgres:14
```

**Notes & Caveats:**

- Compose auto-names containers with project + service + index (here `_db_1`).
- `depends_on` for other services ensures they start after DB. CLI requires scripting.
- Compose does **not wait for DB readiness**; you’d need healthchecks for that.

### Step 4: Start the Cache (Redis)

```yaml
cache:
  image: redis:7
  networks:
    - backendnet
  restart: unless-stopped
```

**CLI Equivalent:**

```bash
docker run -d \
  --name ${PROJECT}_cache_1 \
  --network ${PROJECT}_backendnet \
  --restart unless-stopped \
  redis:7
```

### Step 5: Build and Start API

```yaml
api:
  build: ./api
  environment:
    DATABASE_URL: postgres://user:pass@db:5432/mydb
    REDIS_URL: redis://cache:6379
  depends_on:
    - db
    - cache
  networks:
    - webnet
    - backendnet
  volumes:
    - api_data:/usr/src/app/data
  restart: unless-stopped
```

**CLI Equivalent:**

```bash
### Build API image
docker build -t ${PROJECT}_api ./api

### Run API container
docker run -d \
  --name ${PROJECT}_api_1 \
  --network ${PROJECT}_webnet \
  --network ${PROJECT}_backendnet \
  -e DATABASE_URL=postgres://user:pass@db:5432/mydb \
  -e REDIS_URL=redis://cache:6379 \
  -v ${PROJECT}_api_data:/usr/src/app/data \
  --restart unless-stopped \
  ${PROJECT}_api
```

**Caveats:**

- Multi-network: CLI requires `--network` multiple times to connect to multiple networks.
- `depends_on` ensures Compose starts the DB and cache first, but CLI does not enforce this.

### Step 6: Build and Start Worker

```yaml
worker:
  build: ./worker
  environment:
    DATABASE_URL: postgres://user:pass@db:5432/mydb
    REDIS_URL: redis://cache:6379
  depends_on:
    - api
  networks:
    - backendnet
  restart: unless-stopped
```

**CLI Equivalent:**

```bash
docker build -t ${PROJECT}_worker ./worker

docker run -d \
  --name ${PROJECT}_worker_1 \
  --network ${PROJECT}_backendnet \
  -e DATABASE_URL=postgres://user:pass@db:5432/mydb \
  -e REDIS_URL=redis://cache:6379 \
  --restart unless-stopped \
  ${PROJECT}_worker
```

### Step 7: Build and Start Frontend

```yaml
frontend:
  build: ./frontend
  ports:
    - "3000:3000"
  environment:
    REACT_APP_API_URL: http://api:4000
  depends_on:
    - api
  networks:
    - webnet
  restart: unless-stopped
```

**CLI Equivalent:**

```bash
docker build -t ${PROJECT}_frontend ./frontend

docker run -d \
  --name ${PROJECT}_frontend_1 \
  --network ${PROJECT}_webnet \
  -p 3000:3000 \
  -e REACT_APP_API_URL=http://api:4000 \
  --restart unless-stopped \
  ${PROJECT}_frontend
```

### Step 8: Start Nginx (Reverse Proxy)

```yaml
nginx:
  image: nginx:alpine
  ports:
    - "80:80"
    - "443:443"
  volumes:
    - ./nginx.conf:/etc/nginx/nginx.conf:ro
    - certs:/etc/ssl/certs
  depends_on:
    - frontend
    - api
  networks:
    - webnet
  restart: unless-stopped
```

**CLI Equivalent:**

```bash
docker run -d \
  --name ${PROJECT}_nginx_1 \
  --network ${PROJECT}_webnet \
  -p 80:80 -p 443:443 \
  -v $(pwd)/nginx.conf:/etc/nginx/nginx.conf:ro \
  -v ${PROJECT}_certs:/etc/ssl/certs \
  --restart unless-stopped \
  nginx:alpine
```

**Caveats:**

- Compose’s `depends_on` starts frontend/api first. CLI requires manual ordering.
- SSL certs stored in named volume (`certs`) to persist across container recreation.

### Step 9: Start Prometheus

```yaml
prometheus:
  image: prom/prometheus:latest
  ports:
    - "9090:9090"
  volumes:
    - prometheus_data:/prometheus
  networks:
    - webnet
  restart: unless-stopped
```

**CLI Equivalent:**

```bash
docker run -d \
  --name ${PROJECT}_prometheus_1 \
  --network ${PROJECT}_webnet \
  -p 9090:9090 \
  -v ${PROJECT}_prometheus_data:/prometheus \
  --restart unless-stopped \
  prom/prometheus:latest
```

### Step 10: Start Grafana

```yaml
grafana:
  image: grafana/grafana:latest
  ports:
    - "3001:3000"
  volumes:
    - grafana_data:/var/lib/grafana
  networks:
    - webnet
  depends_on:
    - prometheus
  restart: unless-stopped
```

**CLI Equivalent:**

```bash
docker run -d \
  --name ${PROJECT}_grafana_1 \
  --network ${PROJECT}_webnet \
  -p 3001:3000 \
  -v ${PROJECT}_grafana_data:/var/lib/grafana \
  --restart unless-stopped \
  grafana/grafana:latest
```

### ✅ Summary of Differences

| Compose Feature    | CLI Equivalent                | Caveat / Extra Work                     |
| ------------------ | ----------------------------- | --------------------------------------- |
| `depends_on`       | Manual container ordering     | CLI does not wait for readiness         |
| `volumes:`         | `docker volume create`        | Compose auto-creates named volumes      |
| `networks:`        | `docker network create`       | Must attach containers manually         |
| `restart:`         | `--restart`                   | CLI flag must be repeated per container |
| Multi-network      | Multiple `--network` flags    | Compose simplifies management           |
| Ports mapping      | `-p host:container`           | Must handle manually per service        |
| Build from context | `docker build -t name ./path` | Compose automatically tags images       |
| Auto-naming        | `${PROJECT}_service_index`    | CLI requires `--name`                   |
