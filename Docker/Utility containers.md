# Utility containers

One of the biggest advantages of Docker utility containers is that they let you **use tools without installing them locally**.

You can run tools like:

- **Node.js**, **Python**, **Go**, etc.
- **Database clients** (e.g., `psql`, `mongo`, `mysql`)
- **Build tools** (e.g., `webpack`, `babel`, `esbuild`)
- **CLI utilities** (e.g., `curl`, `jq`, `aws-cli`, `terraform`)

...without installing them on your host machine. You just run them inside a container.

You can run:

- Node 14 for one project
- Node 18 for another
- Python 3.8 for one script, Python 3.12 for another

...all isolated in containers, without affecting your system or other projects.

## Example: Run Node.js Script Without Installing Node

Let’s say you have a script `hello.js`:

```js
console.log("Hello from Node!");
```

You can run it like this using Docker:

```bash
docker run --rm -v "$PWD":/app -w /app node:22 node hello.js
```

This:

- Downloads the official `node:22` image (if not already present)
- Mounts your current directory into the container
- Runs the script using Node 22
- Cleans up the container after it exits

## Other examples using Docker Compose and Dockerfiles

### Example 1: Run a Node.js Script with a Specific Version

#### Project Structure

```bash
node-runner/
├── script.js
├── Dockerfile
└── docker-compose.yml
```

#### `script.js`

```js
console.log("Running Node version:", process.version);
```

#### `Dockerfile`

```Dockerfile
FROM node:18
WORKDIR /app
COPY script.js .
CMD ["node", "script.js"]
```

#### `docker-compose.yml`

```yaml
version: '3.8'
services:
  node-runner:
    build: .
```

#### Run

```bash
docker-compose up --build
```

### Example 2: Use a Utility Container to Format JSON with `jq`

You don’t need to install `jq` locally, just use a container.

#### Project Structure

```bash
jq-runner/
├── data.json
└── docker-compose.yml
```

#### `data.json`

```json
{
  "name": "Alice",
  "age": 30
}
```

#### `docker-compose.yml`

```yaml
version: '3.8'
services:
  jq:
    image: stedolan/jq
    volumes:
      - .:/data
    entrypoint: ["jq", ".", "/data/data.json"]
```

#### Run

```bash
docker-compose up
```

### Example 3: Run Python Script Without Installing Python

#### Project Structure

```bash
python-runner/
├── hello.py
└── docker-compose.yml
```

#### `hello.py`

```python
print("Hello from Python!")
```

#### `docker-compose.yml`

```yaml
version: '3.8'
services:
  python:
    image: python:3.12
    volumes:
      - .:/app
    working_dir: /app
    command: ["python", "hello.py"]
```

### Run

```bash
docker-compose up
```

## Utility Containers and Linux permissions

This is a note under the Udemy course.

I wanted to point out that on a Linux system, the Utility Container idea doesn't quite work as you describe it.  In Linux, by default Docker runs as the "Root" user, so when we do a lot of the things that you are advocating for with Utility Containers the files that get written to the Bind Mount have ownership and permissions of the Linux Root user.  (On MacOS and Windows10, since Docker is being used from within a VM, the user mappings all happen automatically due to NFS mounts.)

So, for example on Linux, if I do the following (as you described in the course):

```Dockerfile
FROM node:14-slim
WORKDIR /app
```

```sh
$ docker build -t node-util:perm .

$ docker run -it --rm -v $(pwd):/app node-util:perm npm init

...

$ ls -la

total 16

drwxr-xr-x  3 scott scott 4096 Oct 31 16:16 ./
drwxr-xr-x 12 scott scott 4096 Oct 31 16:14 ../
drwxr-xr-x  7 scott scott 4096 Oct 31 16:14 .git/
-rw-r--r--  1 root  root   202 Oct 31 16:16 package.json
```

You'll see that the ownership and permissions for the package.json file are "root".  But, regardless of the file that is being written to the Bind Mounted volume from commands emanating from within the docker container, e.g. "npm install", all come out with "Root" ownership.

### Solution 1:  Use  predefined "node" user (if you're lucky)

There is a lot of discussion out there in the docker community (devops) about security around running Docker as a non-privileged user (which might be a good topic for you to cover as a video lecture - or maybe you have; I haven't completed the course yet).  The Official Node.js Docker Container provides such a user that they call "node". 

<https://github.com/nodejs/docker-node/blob/master/Dockerfile-slim.template>

```Dockerfile
FROM debian:name-slim
RUN groupadd --gid 1000 node \
         && useradd --uid 1000 --gid node --shell /bin/bash --create-home node
```

Luckily enough for me on my local Linux system, my "scott" uid:gid is also 1000:1000 so, this happens to map nicely to the "node" user defined within the Official Node Docker Image.

So, in my case of using the Official Node Docker Container, all I need to do is make sure I specify that I want the container to run as a non-Root user that they make available.  To do that, I just add:

```Dockerfile
FROM node:14-slim
USER node
WORKDIR /app
```

If I rebuild my Utility Container in the normal way and re-run "npm init", the ownership of the package.json file is written as if "scott" wrote the file.

```sh
$ ls -la

total 12

drwxr-xr-x  2 scott scott 4096 Oct 31 16:23 ./
drwxr-xr-x 13 scott scott 4096 Oct 31 16:23 ../
-rw-r--r--  1 scott scott 204 Oct 31 16:23 package.json
```

### Solution 2:  Remove the predefined "node" user and add yourself as the user

However, if the Linux user that you are running as is not lucky to be mapped to 1000:1000, then you can modify the Utility Container Dockerfile to remove the predefined "node" user and add yourself as the user that the container will run as:

```Dockerfile
FROM node:14-slim

RUN userdel -r node

ARG USER_ID

ARG GROUP_ID

RUN addgroup --gid $GROUP_ID user

RUN adduser --disabled-password --gecos '' --uid $USER_ID --gid $GROUP_ID user

USER user

WORKDIR /app
```

And then build the Docker image using the following (which also gives you a nice use of ARG):

```sh
docker build -t node-util:cliuser --build-arg USER_ID=$(id -u) --build-arg GROUP_ID=$(id -g) .
```

And finally running it with:

```sh
$ docker run -it --rm -v $(pwd):/app node-util:cliuser npm init

$ ls -la

total 12

drwxr-xr-x  2 scott scott 4096 Oct 31 16:54 ./
drwxr-xr-x 13 scott scott 4096 Oct 31 16:23 ../
-rw-r--r--  1 scott scott  202 Oct 31 16:54 package.json
```

Reference to Solution 2 above: https://vsupalov.com/docker-shared-permissions/

Keep in mind that this image will not be portable, but for the purpose of the Utility Containers like this, I don't think this is an issue at all for these "Utility Containers"
