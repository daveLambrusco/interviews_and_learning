# Docker

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

#### 🔁 Key Differences

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

#### 🧊 Docker Image = Read-Only Blueprint

- It contains everything needed to run your app: code, dependencies, OS libraries, etc.
- Once built, it **does not change**: it's **immutable**.
- You can create multiple containers from the same image, and they’ll all behave the same.

#### 🔄 What to Do When You Change Your Code

If you update your app’s code (e.g., change `index.js`, update `package.json`, etc.), you need to:

1. **Rebuild the Docker image**:

    ```bash
   > docker build -t my-app . #-t is optional: it's used to tag the resulting image as my-app
    ```

   This re-runs the Dockerfile and creates a new image with your updated code.

2. **(Optional) Stop and remove the old container**:

   ```bash
   > docker stop my-app-container
   > docker rm my-app-container
   ```

3. **Run a new container from the updated image**:

   ```bash
   > docker run -p 3000:3000 --name my-app-container my-app
   ```

#### 🛠️ Pro Tip: Use Docker Volumes for Development

If you're actively developing and want to **avoid rebuilding the image every time**, you can mount your code into the container using a volume:

```bash
docker run -v $(pwd):/app -p 3000:3000 node node index.js
```

This way, changes to your local files are reflected immediately inside the container

### Docker images are layer-based

Docker images being **layer-based** means that they are built in a **stack of layers**, where each instruction in your `Dockerfile` (like `FROM`, `COPY`, `RUN`, etc.) creates a **new layer** on top of the previous one.

Let’s break this down:

#### 🧱 What is a Layer?

- A **layer** is a **read-only file system change**.
- Each layer represents a **step** in building the image.
- Layers are **cached** and **reused** to speed up builds.

#### 🔄 Example: Layer Breakdown

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

#### ⚡ Why Layers Matter

- **Efficiency**: If you change only one file, Docker can **reuse previous layers** and rebuild only the changed ones.
- **Caching**: Docker caches each layer. If nothing changes in a layer, it skips rebuilding it.
- **Storage**: Layers are shared between images. If two images use the same base image, they share that layer.

#### 🧠 Optimization

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

🧪 Summary:

| Feature                  | Original Dockerfile | Optimized Dockerfile |
|--------------------------|---------------------|----------------------|
| Caching efficiency       | ❌ Poor             | ✅ Good              |
| Rebuild speed (code only)| 🐢 Slower           | ⚡ Faster             |
| Best practice alignment  | ❌ No               | ✅ Yes               |
