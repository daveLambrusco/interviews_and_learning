# Docker

- [Docker](#docker)
  - [Getting started](#getting-started)
  - [Images vs Containers](#images-vs-containers)
  - [Dockerfile: first example](#dockerfile-first-example)
    - [Diffs between `RUN` and `CMD`](#diffs-between-run-and-cmd)
      - [`RUN` and `CMD`: key Differences](#run-and-cmd-key-differences)
  - [BUILD and RUN an image](#build-and-run-an-image)
    - [Rebuild images](#rebuild-images)
      - [Docker Image = Read-Only Blueprint](#docker-image--read-only-blueprint)
      - [What to Do When You Change Your Code](#what-to-do-when-you-change-your-code)
      - [Pro Tip: Use Docker Volumes for Development](#pro-tip-use-docker-volumes-for-development)
    - [Docker images are layer-based](#docker-images-are-layer-based)
      - [What is a Layer?](#what-is-a-layer)
      - [Example: Layer Breakdown](#example-layer-breakdown)
      - [Why Layers Matter](#why-layers-matter)
      - [Optimization](#optimization)
  - [RUN, START, STOP](#run-start-stop)
    - [`docker run`](#docker-run)
    - [`docker start` / `docker stop`](#docker-start--docker-stop)
    - [Summary Table](#summary-table)
  - [Naming and Tagging](#naming-and-tagging)
    - [Images](#images)
      - [Tagging an Existing Image](#tagging-an-existing-image)
    - [Containers](#containers)
  - [Volumes](#volumes)
    - [Types of Docker Volumes](#types-of-docker-volumes)
    - [Removing anonymous volumes](#removing-anonymous-volumes)
    - [What happens when I create a named volume](#what-happens-when-i-create-a-named-volume)
    - [Anonymous and Named volumes: summary Table](#anonymous-and-named-volumes-summary-table)
    - [Bind mounts](#bind-mounts)
      - [Syntax Example](#syntax-example)
      - [Key Differences from Volumes](#key-differences-from-volumes)
      - [Use Cases for Bind Mounts](#use-cases-for-bind-mounts)
      - [Caution: runtime with Bind Mount](#caution-runtime-with-bind-mount)
  - [ARG and ENV](#arg-and-env)
    - [`ARG` – Build-Time Variables](#arg--build-time-variables)
    - [`ENV` – Runtime Environment Variables](#env--runtime-environment-variables)
    - [`ARG` and `ENV`: key Differences](#arg-and-env-key-differences)
    - [Placing ARG and ENV inside Dockerfile](#placing-arg-and-env-inside-dockerfile)
      - [Common Mistakes](#common-mistakes)
      - [Placing ARG and ENV: summary Table](#placing-arg-and-env-summary-table)
  - [Networks](#networks)
    - [Why Use Docker Networks?](#why-use-docker-networks)
    - [Types of Docker Networks](#types-of-docker-networks)
    - [How Containers Communicate](#how-containers-communicate)
    - [Common Commands](#common-commands)
    - [Connecting multiple containers](#connecting-multiple-containers)
  - [Docker Compose](#docker-compose)
    - [Example](#example)
      - [Example `Dockerfile` for Backend (`backend/Dockerfile`)](#example-dockerfile-for-backend-backenddockerfile)
      - [Example `Dockerfile` for Frontend (`frontend/Dockerfile`)](#example-dockerfile-for-frontend-frontenddockerfile)
    - [`image` vs `build` in Docker Compose](#image-vs-build-in-docker-compose)
      - [Referencing a Local Image](#referencing-a-local-image)
      - [When to Use What?](#when-to-use-what)
    - [Volumes and Bind Mounts: CLI vs Docker Compose](#volumes-and-bind-mounts-cli-vs-docker-compose)
      - [Summary of Differences](#summary-of-differences)
    - [Run the file](#run-the-file)

## Getting started

Hi, so you're a developer.  
Well, you think you are a developer.

Watch this -> <https://youtu.be/DQdB7wFEygo?feature=shared>

## Images vs Containers

🧊 Docker **Image** = Blueprint (Read-Only)

- A **Docker image** is like a **template** or **snapshot** of your application.
- It includes:
  - Your code
  - Dependencies
  - Runtime (like Node.js)
  - OS libraries
- It is **read-only** and **immutable**.
- You build it using a `Dockerfile`.

🚢 Docker **Container** = Running Instance

- A **Docker container** is a **running instance** of an image.
- It’s **mutable** — it can have state, logs, temporary files, etc.
- You can start, stop, restart, and delete containers.
- Containers are **isolated** from each other and from the host system.

🧠 Think of it like an **object** created from a class — it’s the live, working version.

🔁 Analogy

| Concept        | Docker Image             | Docker Container          |
|----------------|--------------------------|---------------------------|
| What it is     | Blueprint / Template     | Running App / Instance    |
| Mutability     | Read-only                | Read-write                |
| Lifecycle      | Built once               | Created, started, stopped |
| Analogy        | Class (in OOP)           | Object (in OOP)           |

---

🧪 Example

```bash
# Build an image from a Dockerfile
docker build -t my-app .

# Run a container from the image
docker run -p 3000:3000 --name my-container my-app
```

- `my-app` is the **image**
- `my-container` is the **container** running that image

## Dockerfile: first example

```Dockerfile
FROM node

WORKDIR app

COPY . /app #or COPY . . because our workdir is /app

RUN npm install

EXPOSE 80

CMD ["node", "index.js"]
```

`FROM node`

- This sets the **base image** (from which we can add our code) for the Docker container.
- `node` refers to the official Node.js image from Docker Hub.
- It includes Node.js and npm pre-installed, so you don’t have to install them manually.

`WORKDIR app`

- This sets the **working directory** inside the container to `/app`.
- All subsequent commands (like `COPY`, `RUN`, etc.) will be executed from this directory.
- If the directory doesn’t exist, Docker will create it.

`COPY . /app`

- This copies all files and folders from your **current directory on your host machine** (where the Dockerfile is located) into the `/app` directory inside the container.
- The `.` means "current directory", and `/app` is the destination inside the container.

`RUN npm install`

- This runs `npm install` inside the container.
- It installs all the dependencies listed in your `package.json` file.

`EXPOSE 80`

- This tells Docker that the container will listen on **port 80** at runtime.
- It doesn’t actually publish the port — that’s done when you run the container using `-p` or `--publish`. Mainly used for **documentation purposes**.

`CMD ["node", "index.js"]`

- This is the **default command** that runs when the container starts.
- It tells Docker to run `node index.js`, which starts your Node.js application.

For more infos and optimization see [layer based images](#docker-images-are-layer-based)

---

### Diffs between `RUN` and `CMD`

🛠️ `RUN` — **Build-time** instruction

- **Purpose**: Executes a command **while building** the Docker image.
- **Used for**: Installing packages, setting up the environment, compiling code, etc.
- **Effect**: The result of the command is saved in the image as a new layer.
- **Example**:

  ```dockerfile
  RUN npm install
  ```

  This installs dependencies and saves them in the image.

🚀 `CMD` — **Runtime** instruction

- **Purpose**: Specifies the **default command** to run when a container starts.
- **Used for**: Running your application or script.
- **Effect**: It does **not** create a new image layer. It just sets the default behavior.
- **Example**:
  
  ```dockerfile
  CMD ["node", "index.js"]
  ```
  
  This runs your app when the container starts.

#### `RUN` and `CMD`: key Differences

| Feature            | `RUN`                          | `CMD`                         |
|--------------------|--------------------------------|-------------------------------|
| When it runs       | During image **build**         | During container **runtime**  |
| Purpose            | Set up the image               | Set default container behavior|
| Creates layer?     | ✅ Yes                         | ❌ No                         |
| Can be overridden? | ❌ No (unless rebuilt)         | ✅ Yes (with `docker run`)    |
| Example use        | Install dependencies           | Start the app                 |

You can only have **one `CMD`** in a Dockerfile. If you write multiple, only the **last one** is used. But you can have **many `RUN`** commands.

## BUILD and RUN an image

```bash
docker build . #executed in the project folder

...

>> Successfully built *imageId*

docker run -p 3000:80 *imageId* #invokes CMD instruction
```

`-p 3000:80` -> to expose localPort:internalDockerPort
When I make a request to my local port 3000, it's redirected towards internal container's 80 port

```bash
> docker ps #lists running containers (use -a to see all)

> docker stop containerName
```

### Rebuild images

A Docker image is like a **read-only template** or **snapshot** of your application and its environment. Here's how it works and what to do when your code changes:

#### Docker Image = Read-Only Blueprint

- It contains everything needed to run your app: code, dependencies, OS libraries, etc.
- Once built, it **does not change**: it's **immutable**.
- You can create multiple containers from the same image, and they’ll all behave the same.

#### What to Do When You Change Your Code

If you update your app’s code (e.g., change `index.js`, update `package.json`, etc.), you need to:

1. **Rebuild the Docker image**:

    ```bash
   > docker build -t my-app . 
    ```

   This re-runs the Dockerfile and creates a new image with your updated code.  
   `-t` is an optional parameter: it's used to [tag](#naming-and-tagging) the resulting image as my-app

2. **(Optional) Stop and remove the old container**:

   ```bash
   > docker stop my-app-container
   > docker rm my-app-container
   ```

3. **Run a new container from the updated image**:

   ```bash
   > docker run -p 3000:3000 --name my-app-container my-app
   ```

#### Pro Tip: Use Docker Volumes for Development

If you're actively developing and want to **avoid rebuilding the image every time**, you can mount your code into the container using a [volume](#volumes):

```bash
docker run -v $(pwd):/app -p 3000:3000 node node index.js
```

This way, changes to your local files are reflected immediately inside the container

### Docker images are layer-based

Docker images being **layer-based** means that they are built in a **stack of layers**, where each instruction in your `Dockerfile` (like `FROM`, `COPY`, `RUN`, etc.) creates a **new layer** on top of the previous one.

Let’s break this down:

#### What is a Layer?

- A **layer** is a **read-only file system change**.
- Each layer represents a **step** in building the image.
- Layers are **cached** and **reused** to speed up builds.

#### Example: Layer Breakdown

Given this Dockerfile:

```dockerfile
FROM node              # Layer 1: base image
WORKDIR /app           # Layer 2: sets working directory
COPY . /app            # Layer 3: copies your code
RUN npm install        # Layer 4: installs dependencies
CMD ["node", "index.js"] # Layer 5: sets default command
```

Docker builds it like this:

1. **Layer 1**: Pulls the Node.js base image.
2. **Layer 2**: Adds metadata for the working directory.
3. **Layer 3**: Adds your app files.
4. **Layer 4**: Installs dependencies.
5. **Layer 5**: Adds the default command to run.

#### Why Layers Matter

- **Efficiency**: If you change only one file, Docker can **reuse previous layers** and rebuild only the changed ones.
- **Caching**: Docker caches each layer. If nothing changes in a layer, it skips rebuilding it.
- **Storage**: Layers are shared between images. If two images use the same base image, they share that layer.

#### Optimization

To take advantage of caching put commands that **change less often** (like `RUN npm install`) **before** commands that change frequently (like `COPY .`). This avoids invalidating the cache too early.

```Dockerfile
FROM node

WORKDIR /app

COPY package*.json /app

RUN npm install

COPY . /app

EXPOSE 80

CMD ["node", "server.js"]
```

✅ **Optimization Benefits:**

1. **Layer Caching**:
   - By copying `package.json` and `package-lock.json` before running `npm install`, Docker can **cache the `npm install` layer**.
   - If your code changes but `package.json` doesn’t, Docker **won’t re-run `npm install`**, saving time.

2. **Fewer Rebuilds**:
   - If you change only your app code (e.g., `.js` files), Docker will reuse the cached layers up to `COPY package.json`.

3. **Cleaner Dependency Management**:
   - Keeps dependency installation separate from app code, which is a good practice.

---

🧪 Summary:

| Feature                  | Original Dockerfile | Optimized Dockerfile |
|--------------------------|---------------------|----------------------|
| Caching efficiency       | ❌ Poor             | ✅ Good              |
| Rebuild speed (code only)| 🐢 Slower           | ⚡ Faster             |
| Best practice alignment  | ❌ No               | ✅ Yes               |

## RUN, START, STOP

### `docker run`

- Creates and starts a **new container.**
- Syntax: `docker run [OPTIONS] IMAGE [COMMAND]`
- You use this when you're starting a container for the first time.

**Key behaviors:**

- If the image doesn't exist locally, Docker pulls it.
- A new container is created and started based on that image.
- Unless specified, it runs in **attached mode** (i.e., logs/output appear in your terminal).

**Modes:**

- Attached mode (default):
  - You see the container's output in your terminal.
  - Ctrl+C stops the container.
- Detached mode (`-d`):
  - The container runs in the background.
  - You get the container ID as output, but no logs are shown unless you use `docker logs`.

**Example:**

```bash
docker run -it ubuntu bash   # attached mode (interactive shell)
docker run -d nginx          # detached mode (background Nginx server)
```

### `docker start` / `docker stop`

- Used for **existing (previously created)** containers.
- `docker start`: Starts a *stopped* container.
- `docker stop`: Gracefully stops a *running* container.

These **do not** create new containers, just manage the lifecycle of existing ones.

**Modes with `start`:**

- By default, `docker start` runs in **detached mode**.
- To run in **attached mode**, use the `-a` option:

  ```bash
  docker start -a <container_id_or_name>
  ```

**Examples:**

```bash
docker stop my_nginx       # Stop the container named my_nginx
docker start my_nginx      # Start it again (in detached mode)
docker start -a my_nginx   # Start in attached mode, show output
```

You can attach to an existing container using `docker attach containerName`.
Another way (used to read logs) is to run `docker logs -f containerName`. This command attaches to the container and follows printed logs.

---

### Summary Table

| Command        | Creates New Container | Starts Container | Can Run in Attached Mode | Can Run in Detached Mode |
| -------------- | --------------------- | ---------------- | ------------------------ | ------------------------ |
| `docker run`   | ✅ Yes                 | ✅ Yes            | ✅ Yes (default)          | ✅ Yes (`-d`)             |
| `docker start` | ❌ No                  | ✅ Yes            | ✅ Yes (`-a`)             | ✅ Yes (default)          |
| `docker stop`  | ❌ No                  | ❌ No (it stops)  | ❌ No                     | ❌ No                     |

---

## Naming and Tagging

### Images

📌 Basic Syntax

```bash
docker build -t [name]:[tag] .
```

- `name`: The repository name for the image
- `tag`: An optional label (default is `latest` if omitted)

✅ Examples

```bash
docker build -t myapp:1.0 .
docker build -t username/myapp:dev .
```

This tags the image as `username/myapp` with a `dev` version.

#### Tagging an Existing Image

You can tag an existing image using:

```bash
docker tag [source_image]:[source_tag] [target_image]:[target_tag]
```

✅ Example

```bash
docker tag myapp:latest myapp:2.0
```

This creates another reference (tag) to the same image.

### Containers

📌 Running with a Custom Name

```bash
docker run --name [container_name] [image_name]:[tag]
```

✅ Examples

```bash
docker run --name webserver myapp:latest
```

This creates a container named `webserver` from the `myapp` image.

## Volumes

In Docker, **volumes** are a mechanism for **persisting data** generated and used by containers. They allow data to exist **outside the container's lifecycle**, meaning the data remains even if the container is deleted or recreated.

Volumes are stored in a part of the host filesystem managed by Docker (`/var/lib/docker/volumes/` on Linux). They are the preferred mechanism for persisting data because they:

- Are easier to back up or migrate.
- Can be shared between multiple containers.
- Are managed by Docker (you don’t need to worry about host paths).
- Work on both Linux and Windows containers.

### Types of Docker Volumes

1) **Anonymous Volumes**
  
   - Created **without a name**.
   - Docker assigns them a random name (e.g., `f3c1e2d3b4a5...`).
   - Typically used when you mount a volume without specifying a name:
  
   ```bash
   docker run -v /app/data my-image
   ```

   - **Use case**: Temporary storage where you don’t need to reference the volume again.

2) **Named Volumes**

   - Created with a **specific name**.
   - Can be reused across multiple containers.

     ```bash
     docker volume create mydata
     docker run -v mydata:/app/data my-image
     ```

   - **Use case**: Persistent storage that you want to manage and reuse.

### Removing anonymous volumes

Anonymous volumes are removed automatically, when a container is removed **ONLY** when you start / run a container with the `--rm` option.

If you start a container without that option, the anonymous volume would NOT be removed, even if you remove the container (with `docker rm ...`).

Still, if you then re-create and re-run the container (i.e. you run `docker run ...` again), **a new anonymous volume will be created**. So even though the anonymous volume wasn't removed automatically, it'll also not be helpful because a different anonymous volume is attached the next time the container starts (i.e. you removed the old container and run a new one).

Now you just start piling up a bunch of unused anonymous volumes. You can clear them via `docker volume rm VOL_NAME` or `docker volume prune.`

### What happens when I create a named volume

When you run the following commands on your local machine:

```bash
docker volume create mydata
docker run -v mydata:/app/data my-image
```

here’s what happens step by step:

🧱 Step 1: `docker volume create mydata`

- Docker creates a **named volume** called `mydata`.
- This volume is stored in Docker’s default volume directory:
  
   ```bash
  /var/lib/docker/volumes/mydata/_data
  ```

- It’s an **empty directory** at this point, managed by Docker.
- You can inspect it with:

  ```bash
  docker volume inspect mydata
  ```

🚀 Step 2: `docker run -v mydata:/app/data my-image`

- Docker starts a new container from the image `my-image`.
- It **mounts the `mydata` volume** to the container’s internal path `/app/data`.
- Inside the container, anything written to `/app/data` is actually stored in the `mydata` volume on your host.
- This means:
  - Data persists even if the container is deleted.
  - You can reuse the volume in other containers.

🗂️ On Your Local Machine

- A folder is created at:
  
  ```bash
  /var/lib/docker/volumes/mydata/_data
  ```

- This folder is **not directly visible** in your project directory unless you explicitly mount it.
- Docker manages this folder, and it’s used to store the data written by the container to `/app/data`.

### Anonymous and Named volumes: summary Table

| Feature            | Anonymous Volume         | Named Volume             |
|--------------------|--------------------------|--------------------------|
| Name               | Randomly generated       | User-defined             |
| Reusability        | Hard to reuse             | Easy to reuse            |
| Use case           | Temporary, one-off tasks | Persistent, shared data  |
| Management         | Harder to manage          | Easier to manage         |

### Bind mounts

**Bind mounts** allow you to **mount a specific file or directory from your host machine** into a container. Unlike Docker-managed volumes, bind mounts give you **direct control** over the exact path on the host.  
They are a commonly used feature, especially in **development environments** and certain production use cases where direct access to host files is needed.

#### Syntax Example

```bash
docker run -v /ABSOLUTE/path/on/host:/path/in/container my-image
```

- `/ABSOLUTE/path/on/host`: A directory or file on your local machine.
- `/path/in/container`: Where it will appear inside the container.

#### Key Differences from Volumes

| Feature              | Bind Mounts                          | Docker Volumes                      |
|----------------------|--------------------------------------|-------------------------------------|
| Managed by Docker    | ❌ No                                | ✅ Yes                               |
| Host path control    | ✅ You specify the exact path         | ❌ Docker manages the path           |
| Portability          | ❌ Less portable (host-specific)      | ✅ More portable                     |
| Use case             | Development, config files, logs      | Persistent app data, databases      |
| Security             | ⚠️ Less secure (full host access)     | ✅ More secure (Docker-managed)      |

#### Use Cases for Bind Mounts

- **Live code editing**: Mount your source code into the container so changes reflect instantly.
- **Accessing config files**: Use host config files inside the container.
- **Log collection**: Write logs from the container to a host directory.

#### Caution: runtime with Bind Mount

Bind mounts can **break** if the host path doesn’t exist, if Docker doesn't have access to the resource folder or some parts/files/modules are missing.

Your `Dockerfile` does:

  ```Dockerfile
  COPY . /app
  RUN npm install
  ```

So when you build the image:

- Your code is copied to `/app`.
- `npm install` installs dependencies into `/app/node_modules`.

Then you run the container with a bind mount:

  ```bash
  docker run -v $(pwd):/app my-node-app
  ```

😬 What if your local folder **doesn't have `node_modules`**?

- Then `/app/node_modules` appears **empty** in the container
- Your app will crash with errors like `Cannot find module 'express'` or similar

✅ Solutions

1. **Use a Separate Volume for `node_modules`**

    Keep code from your host, but let Docker manage `node_modules`:

    ```bash
    docker run -v $(pwd):/app -v /app/node_modules my-node-app
    ```

    - First volume mounts your code
    - Second volume preserves container's `node_modules` so they're not overridden

    🟢 **Recommended for development**

2. **Install `node_modules` into Local Folder (One-Time)**

    ```bash
    docker run -v $(pwd):/app node:22 npm install
    ```

    - Uses a Node container to run `npm install` into your local project folder
    - Then you can run:

      ```bash
      docker run -v $(pwd):/app my-node-app
      ```

    🟡 **Works well but pollutes local environment**

3. **Skip Bind Mount for Production**

    If you're building for production and don’t need to sync local code:

    ```bash
    docker run my-node-app
    ```

    - No volumes
    - Uses fully built image with `COPY` and `node_modules`

    🟢 **Recommended for deployment**

4. **Dev-Only: Install on Start**

    Change `CMD` to install dependencies when the container starts:

    ```Dockerfile
    CMD ["sh", "-c", "npm install && npm start"]
    ```

    This works even with a bind mount, but slows down startup and isn’t ideal for production.

    🔴 **Not recommended for production**

---

🚧 Summary Table

| Use Case                    | What to Do                                                                      |
| --------------------------- | ------------------------------------------------------------------------------- |
| Dev with live code sync     | `-v $(pwd):/app -v /app/node_modules`                                           |
| One-time install            | `docker run -v $(pwd):/app node:22 npm install`                                 |
| Pure production image       | No bind mount — just run `docker run my-node-app`                               |
| Dev, no local node\_modules | Avoid running container without `-v /app/node_modules` or install locally first |

## ARG and ENV

When working with Docker, two commonly used instructions in the Dockerfile are `ARG` and `ENV`. Both are used to define variables, but they serve different purposes and exist at different stages of the Docker lifecycle.

### `ARG` – Build-Time Variables

The `ARG` instruction defines a variable that is **only available during the image build process**. It allows you to parameterize parts of your Dockerfile so that different builds can behave differently without changing the Dockerfile itself.

Characteristics

- **Scope**: Limited to the build stage (`docker build`).
- **Not preserved** in the final image.
- **Useful for** customizing things like base image versions, optional build tools, or credentials for temporary use during build (e.g., cloning a private repo).
- **Can have default values**.
- **Can be overridden** using the `--build-arg` flag when building.

✅ Example

```Dockerfile
ARG VERSION=1.0
FROM node:${VERSION}
```

```sh
docker build --build-arg VERSION=20 -t my-app .
```

In this example, the base Node.js version can be changed at build time.

---

### `ENV` – Runtime Environment Variables

The `ENV` instruction defines environment variables that are **embedded into the image** and available to the application **at runtime** (when a container starts). These variables persist in the image and can be used by scripts, applications, and even during the build process after being defined.

Characteristics

- **Scope**: Available during both build and runtime.
- **Persisted** in the final image and visible inside containers.
- **Used for** configuring application behavior, defining paths, ports, credentials (note: not secure), etc.
- **Can be overridden** at runtime with the `-e` or `--env` flag when running a container.

✅ Example

```Dockerfile
ENV NODE_ENV=production
```

```sh
docker run -e NODE_ENV=development my-app
```

In this case, the default `NODE_ENV` is `production`, but it can be overridden when the container is started.

---

### `ARG` and `ENV`: key Differences

| Feature      | `ARG`                      | `ENV`                         |
| ------------ | -------------------------- | ----------------------------- |
| Availability | Build-time only            | Build-time and runtime        |
| Visibility   | Not available in container | Available inside container    |
| Persistence  | Not saved in image         | Saved in image                |
| Override     | `--build-arg` (build)      | `-e` or `--env` (run)         |
| Security     | Safer (not in final image) | Not secure (visible in image) |

### Placing ARG and ENV inside Dockerfile

Each instruction in a Dockerfile creates a new **layer**. Docker caches these layers to avoid rebuilding unchanged parts. Therefore, **placing variable declarations properly** can improve cache use and reduce rebuild time.

**Place `ARG` as early as possible**, especially **before `FROM`**, if you want to use it in the `FROM` instruction. If used only later in the build, place it right before it’s needed.

✅ Example

```Dockerfile
# Define ARG before FROM if used in FROM
ARG NODE_VERSION=22
FROM node:${NODE_VERSION}

# Another ARG used later
ARG APP_NAME
RUN echo "Building ${APP_NAME}"
```

> ⚠️ Note: Only `ARG`s defined **before `FROM`** can be used in the `FROM` instruction.

Place `ENV` **just before it's used**, especially if it affects build steps (e.g., a `RUN` command). If it’s for **application runtime config**, place it **after all build steps**, close to the final stage.

✅ Example

```Dockerfile
FROM node:22

# Used during build (affects RUN)
ENV PATH=/opt/app/bin:$PATH
RUN mkdir -p /opt/app && echo $PATH

# Used at runtime (final image)
ENV NODE_ENV=production
CMD ["node", "app.js"]
```

#### Common Mistakes

- **Don't place unnecessary `ENV` too early**: If it changes frequently, early placement may invalidate cached layers.
- **Don’t overuse `ARG`**: They aren't available at runtime—don’t rely on them for environment config.
- **Don't redefine variables** unless necessary—each redefinition creates a new layer.

#### Placing ARG and ENV: summary Table

| Instruction | Best Placement                                                     | Reason                                             |
| ----------- | ------------------------------------------------------------------ | -------------------------------------------------- |
| `ARG`       | Before `FROM` (if used there) or before use                        | Affects base image or build logic                  |
| `ENV`       | Before build step (if used during build) or late (if runtime only) | Keeps cache effective; avoids unnecessary rebuilds |

## Networks

A **Docker network** is a virtual network that allows containers to communicate with each other and with external systems. Each container can be connected to one or more networks, enabling flexible communication paths and isolation.

### Why Use Docker Networks?

- **Inter-container communication**: So containers can talk to each other directly.
- **Isolation**: Separate environments for different applications or stages.
- **Security**: Limit exposure of services to only those that need access.
- **Scalability**: Helps services discover and connect to each other dynamically.

### Types of Docker Networks

Docker supports several built-in network drivers, each serving different purposes:

| Network Type | Description                                                                                  |
| ------------ | -------------------------------------------------------------------------------------------- |
| **bridge**   | Default for standalone containers. Isolated network on the Docker host.                      |
| **host**     | Removes network isolation between container and host. Container shares host's network stack. |
| **macvlan**  | Assigns a MAC address to a container for direct access to the physical network.              |
| **none**     | No network connectivity. Used for custom network stacks or extreme isolation.                |
| **overlay**  | Used in Docker Swarm. Allows containers across multiple hosts to communicate securely.       |

---

### How Containers Communicate

1. **Same Bridge Network**:

   - Docker assigns an internal IP.
   - Containers can reference each other **by container name**.

2. **Different Networks**:

   - You must connect containers to a shared network.
   - External access may require published ports (`-p` or `--publish`).

3. **Docker Compose**:

   - Automatically creates a user-defined bridge network.
   - Containers can talk using service names as DNS names.

### Common Commands

```bash
# List all networks
docker network ls

# Inspect a network
docker network inspect <network_name>

# Create a custom bridge network
docker network create my_custom_network

# Run a container in a specific network
docker run -d --network=my_custom_network --name=my_container nginx

# Connect an existing container to a network
docker network connect my_custom_network my_container

# Disconnect a container from a network
docker network disconnect my_custom_network my_container
```

### Connecting multiple containers

🔗 Step 1: Create a Shared Network

You can use a user-defined bridge network for easy name-based container communication.

```bash
docker network create my_shared_network
```

📦 Step 2: Run Containers in That Network

Let's say you're running a **Node.js app**, a **MongoDB** database, and a **Redis** cache.

```bash
# Run MongoDB
docker run -d --name mongodb --network=my_shared_network mongo

# Run Redis
docker run -d --name redis --network=my_shared_network redis

# Run your Node.js app
docker run -d --name node-app --network=my_shared_network node-app-image
```

> ✅ Because they are in the same user-defined bridge network, they can resolve each other by container name (e.g., `"mongodb"` or `"redis"`). There's **no need to publish the ports**.

📍 Step 3: Refer to Services by Name in Node.js

You don't need to use IP addresses. Docker provides **internal DNS resolution**, so you can connect like this:

🟦 MongoDB with `mongoose`

```js
import mongoose from 'mongoose';

await mongoose.connect('mongodb://mongodb:27017/mydb', {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});
```

🟥 Redis with `ioredis`

```js
import Redis from 'ioredis';

const redis = new Redis({
  host: 'redis',
  port: 6379,
});
```

> 🔁 The hostnames `mongodb` and `redis` come from the container **names** on the shared Docker network.

## Docker Compose

**Docker Compose** is a tool used for defining and running multi-container Docker applications. With Docker Compose, you can use a **YAML file** to configure your application's services, networks, and volumes, and then start everything with a single command: `docker-compose up`.

Docker Compose:

- **Simplifies multi-container setups** (e.g., web server + database).
- **Centralized configuration** in a single file.
- **Easier development and testing** environments.

---

### Example

Let’s say you have a basic web application that uses

- **Angular** (frontend)
- **Node.js/Express** (backend)
- **MongoDB** (database)

📁 Project Structure

```bash
my-fullstack-app/
│
├── docker-compose.yml
├── backend/
│   └── Dockerfile
├── frontend/
│   └── Dockerfile
└── mongo-data/  (volume for MongoDB)
```

🧾 `docker-compose.yml`

```yaml
services:
  mongo:
    image: mongo
    container_name: mongo
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db

  backend:
    build: ./backend #executes Dockerfile inside backend folder
    container_name: backend
    ports:
      - "3000:3000"
    environment:
      - MONGO_URL_EQUAL=mongodb://mongo:27017/mydb
      - MONGO_URL_COLON: mongodb://mongo:27017/mydb #equivalent to the first syntax
    env_file: #to store env variables in a separate file
      - ./env/mongo.env
    depends_on:
      - mongo #ensures MongoDB starts before the backend.

  frontend:
    build: ./frontend
    container_name: frontend
    ports:
      - "4200:4200"
    volumes:
      - ./frontend/src:/app/src #to reflect local changes inside the container
    stdin_open: true     # equivalent to -i (interactive)
    tty: true            # equivalent to -t (allocate a pseudo-TTY
    depends_on:
      - backend

volumes:
  mongo-data: #yes, this is the correct syntax (DON'T REMOVE THE COLON!)
```

This setup:

- Spins up MongoDB, Node.js, and Angular containers.
- Connects them via Docker’s internal network.
- Ensures proper startup order.
- Exposes ports so you can access:
  - Angular at <http://localhost:4200>
  - Node.js API at <http://localhost:3000>
  - MongoDB at localhost:27017

There's no need to specify `--rm` (remove when shut down) or `-d` (detach mode) in this file.  
Containers are automatically removed when shut down and the `-d` can be specified inside the [run](#run-the-file) command

#### Example `Dockerfile` for Backend (`backend/Dockerfile`)

```Dockerfile
FROM node:22

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000
CMD ["npm", "start"]
```

#### Example `Dockerfile` for Frontend (`frontend/Dockerfile`)

```Dockerfile
FROM node:22

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 4200
CMD ["npm", "start"]
```

### `image` vs `build` in Docker Compose

| Key | `image` | `build` |
|-----|--------|--------|
| **Purpose** | Use a pre-built Docker image | Build a Docker image from a Dockerfile |
| **Usage** | When the image is already available (locally or on Docker Hub) | When you want to build a custom image from source code |
| **Example** | `image: node:22` | `build: ./backend` |
| **Flexibility** | Less flexible (you use what's already built) | More flexible (you define how the image is built) |

#### Referencing a Local Image

If you've already built a Docker image locally (e.g., with `docker build -t my-custom-image .`), you can reference it in your `docker-compose.yml` like this:

```yaml
services:
  myservice:
    image: my-custom-image
```

> ⚠️ Just make sure the image exists locally before running `docker-compose up`, or it will try to pull it from Docker Hub and fail.

#### When to Use What?

- Use **`image`** when:
  - You want to use a standard image (like `mongo`, `node`, `nginx`, etc.).
  - You’ve already built the image manually and want to reuse it.

- Use **`build`** when:
  - You have a custom app with a `Dockerfile`.
  - You want Docker Compose to handle building the image for you.

### Volumes and Bind Mounts: CLI vs Docker Compose

In **Docker CLI** you specify paths directly in the `-v` or `--mount` flag:

```bash
docker run -v /absolute/host/path:/container/path myimage
```

- The **host path must be absolute**.
- You can’t use relative paths like `./data`.

In **Docker Compose** you can use **relative paths**, and they are **relative to the location of the `docker-compose.yml` file**:

```yaml
services:
  app:
    image: myimage
    volumes:
      - ./data:/app/data  # relative path
      - /absolute/path:/app/config  # absolute path
```

- `./data` is relative to the directory where `docker-compose.yml` is located.
- `/absolute/path` works the same as in CLI.

#### Summary of Differences

| Feature            | Docker CLI                      | Docker Compose                     |
|--------------------|----------------------------------|-------------------------------------|
| Relative paths     | ❌ Not allowed                   | ✅ Allowed (relative to compose file) |
| Absolute paths     | ✅ Required                      | ✅ Allowed                           |
| Named volumes      | ✅ Supported                     | ✅ Supported                         |

### Run the file

The `docker-compose up` command **builds, (re)creates, starts, and attaches** to containers for all services defined in your `docker-compose.yml`.

🔧 Basic Usage

```bash
docker-compose up
```

This will:

- Build images (if not already built)
- Create containers
- Start services
- Stream logs to your terminal

✅ Common Flags

| Flag | Description |
|------|-------------|
| `-d` | Run in **detached mode** (in the background) |
| `--build` | Force rebuild of images before starting |
| `--force-recreate` | Recreate containers even if nothing changed |
| `--remove-orphans` | Remove containers not defined in the current `docker-compose.yml` |
| `--no-deps` | Don’t start linked services (useful for running a single service) |

---

The `docker-compose down` command **stops and removes** everything created by `docker-compose up`, including:

- Containers
- Networks
- Volumes (optional)
- Images (optional)

🔧 Syntax

```bash
docker-compose down [options]
```

✅ Common Flags

| Flag | Description |
|------|-------------|
| `-v` | Also remove named volumes declared in `volumes:` |
| `--rmi all` | Remove all images used by any service |
| `--remove-orphans` | Remove containers not defined in the current `docker-compose.yml` |
