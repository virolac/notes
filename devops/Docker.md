# Docker

## What are containers?
Containers are standardized, executable components that combine application source code with the operating system (OS)
libraries and dependencies required to run that code in any environment.

They simplify development and delivery of distributed appications.

Containers are made possible by process isolation and virtualization capabiities built into the **Linux** kernel. These
capabilities--such as *control groups* (Cgroups) for allocating resources among processes, and *namespaces* for
restricting a process's access or visibility into other resources or areas of the system--enable multiple application
components to share the resources of a single instance of the host operating system in much the same way that a
hypervisor enables multiple virtual machines to share the CPU, memory and other resources of a single hardware server.

This is also the main difference between containers and virtual machines. Virtual machines virtualize an entire machine
down to the hardware layers, whereas containers only virtualize software layers above the operating system level.

## Why are they so popular?
- They are light weight. Unlike **VMs**, containers don't carry the payload of an entire OS instance and hypervisor.
  They include only the OS processes and dependencies necessary to execute the code. Container sizes are measured in
  megabytes (vs gigabytes for some **VMs**), make better use of hardware capacity, and have faster startup times.

- Containerized applications can be written once and run anywhere. And compared to **VMs** containers are faster and
  easier to deploy, provision and restart. This makes them ideal for use in *continuous integration* and *continuous
  delivery* (**CI/CD**) pipelines and a better fit for development teams adopting *Agile* and *DevOps* practices.

- They have greater resource efficiency compared to **VMs**. With containers, developers can run several times as many
  copies of an application on the same hardware as they can using **VMs**, which can reduce cloud spending.

## What is Docker?
- An open source platform that enables developers to build, deploy, run, update and manage *containers*.

- It is so popular, that *"Docker"* and *"containers"* are used interchangeably, even though the first container-related
  technologies were available for years--even dacades--before **Docker** was released to the public in 2013

- **Docker** lets developers access the native containerization capabilities of **Linux** using simple commands, and
  automate them through a work-saving application programming interface (**API**)

- Its containers are portable, which means they can run without modification across any desktop, data center and cloud
  environment

- Existing containers can be used as base images--essentially like templates--for building new containers

- **Docker** can also track versions of a container image, roll back to previous versions, and trace who built a version
  and how

- Developers have access to an [**open-source registry**](https://hub.docker.com/) containing thousands of
  user-contributed containers

## Glossary
- `container` - an isolated environment, with its own processes, network interfaces, volume mounts, etc.
- `image` - a read-only immutable template that defines how a container will be realized
- `registry`- a central repository of **Docker** images

## Docker Engine
**Docker Engine** is an open source containerization technology for building and containerizing our applications. It
acts as a client-server application with:

- A server with a long-running daemon process: `dockerd`
- **APIs** which specify interfaces that programs can use to talk to and instruct the **Docker** daemon
- A **CLI**, `docker`, which uses the **Docker APIs** to control or interact with the daemon through scripting or direct
  [CLI commands](#cli-commands)

The daemon creates and manages **Docker** objects such as images, containers, networks, and volumes.

## CLI commands
- ### `pull`
  The `pull` command can be used to download an image from a registry without creating and running a container.

  The full path to the image can be specified in the form `<registry>/<account>/<image>`:

  ```sh
  docker pull docker.io/nginx/nginx
  ```

  When we pull an official image from [**Docker Hub**](https://hub.docker.com/), we can omit the registry and account
  name:

  ```sh
  docker pull nginx
  ```

- ### `run`
  The `run` command can be used to start a container from an image:

  ```sh
  docker run nginx
  ```

  If the image is not present on the host, it will go out to **Docker Hub** and [`pull`](#pull) the image down. In
  subsequent runs, it will use the same image.

  #### Run a container and specify a command
  ```sh
  docker run ubuntu sleep 5
  ```

  #### Specify a version using a tag
  ```sh
  docker run redis:4.0
  ```

  <small>The default tag is called `latest`.</small>

  #### Run a container in detached mode
  We can run a container in the background and print its **ID** using the `--detach / -d` flag:

  ```sh
  docker run -d nginx
  ```

  #### Keep STDIN open even if not attached
  By default a container does not listen to standard input. Even if we attach to its console, it is not able to read
  any input from us. It doesn't have a terminal to read input from. It runs in a *non-interactive mode*.

  We can map the standard input of our host to the Docker container using the `--interactive / -i` flag:

  ```sh
  docker run -i ubuntu
  ```

  #### Allocate a tty
  We can allocate a pseudo terminal for the container process using the `--tty / -t` flag:

  ```sh
  docker run -it ubuntu
  ```

  #### Assign a name to a container
  We can name a container using the `--name` flag:

  ```sh
  docker run -d --name=redis redis
  ```

  #### Set working directory
  The default working directory for running binaries within a container is the root directory (`/`). It is possible to
  change it using the `--workdir / -w` flag:

  ```sh
  docker run -w /path/to/dir -i -t ubuntu pwd
  ```

  #### Set environment variables
  We can use the `--env / -e` flag to set or override simple (non-array) environment variables in the container we're
  running:

  ```sh
  docker run -e MYVAR1 --env MYVAR2=foo ubuntu bash
  ```

  #### Limit the CPU resources available to a container
  We can specify how much of the available CPU resources a container can use using the `--cpus` flag:

  ```sh
  docker run -it --cpus=".5" ubuntu /bin/bash
  ```

  #### Limit the amount of memory a container can utilize
  We can specify the maximum amount of memory a container can use using the `--memory / -m` flag:

  ```sh
  docker run -it -m 2GB ubuntu:22.04 /bin/bash
  ```

  #### Publish a container port
  We can bind a container port to a port of the host machine using the `--publish / -p` flag:

  ```sh
  docker run -p 127.0.0.1:80:8080/tcp ubuntu bash
  ```

  #### Connect a container to a network
  ```sh
  docker run -itd --network=host nginx
  ```

- ### `attach`
  We can attach local standard input, output, and error streams to a running container using the `attach` command:

  ```sh
  docker attach <container-id>
  ```

  It is forbidden to redirect the standard input of a `docker attach` command while attaching to a **TTY**-enabled
  container (using the `-i` and `-t` options).

- ### `ps`
  The `ps` command lists all running containers with some basic information about them, such as the container **ID**,
  the name of the image we used to run it, the current status, and the name of the container.

  **Docker** automatically asigns a container **ID** and name to our containers.

  To see a list of all containers, even those that aren't running, we can use the `--all / -a` flag:

  ```sh
  docker ps -a
  ```

  This will output all running as well as previously stopped or exited containers.

- ### `stop`
  The `stop` command can be used to stop a container by **ID** or name:

  ```sh
  docker stop f4h2n5hvwvb
  ```

  If the container is running, the command will stop it and print its name. Otherwise, it will print a message
  indicating that no containers with that **ID** or name are running.

- ### `rm`
  The `rm` command can be used to permanently remove a stopped or exited container:

  ```sh
  docker rm redis
  ```

  On success, it will print the container name.

- ### `rmi`
  We can remove one or more images using the `rmi` command:

  ```sh
  docker rmi ubuntu:latest
  ```

  Before we remove an image, we must first remove all its dependent containers.

- ### `images`
  The `images` command outputs a list of available images and their sizes:

  ```sh
  docker images
  ```

- ### `exec`
  We can execute a command in a running container using the `exec` command:

  ```sh
  docker exec it mycontainer sh
  ```

- ### `version`
  The `version` command can be used to show the **Docker** version information:

  ```sh
  docker version
  ```

- ### `inspect`
  The `inspect` command returns low-level information on **Docker** objects:

  ```sh
  docker inspect
  ```

  By default, it will render results in a **JSON** array.

- ### `logs`
  We can fetch the logs of a container using the `logs` command:

  ```sh
  docker logs --follow --until=2s test
  ```

- ### `history`
  We can show the history of an image using the `history` command:

  ```sh
  docker history apache
  ```

## `Dockerfile`
**Docker** can build images automatically by reading the instructions from a `Dockerfile`. A `Dockerfile` is a text
document that contains all the commands a user could call on the command line to assemble an image.

**Docker** runs instructions in a `Dockerfile` in order.

A `Dockerfile` **must begin with a `FROM` instruction**. That's because every image must be based off of another image.

### Format
Here is the format of the `Dockerfile`:

```Dockerfile
# Comment
INSTRUCTION arguments
```

The instruction is not case-sensitive. However, convention is for them to be **UPPERCASE** to distinguish them from
arguments more easily.

**Example:**

```Dockerfile
FROM Ubuntu

RUN apt-get update
RUN apt-get install python

RUN pip install flask
RUN pip install flask-mysql

COPY . /opt/source-code

ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run
```

### Instructions
- ```Dockerfile
  FROM <image>
  ```

  Initializes a new build stage and sets the base image for subsequent instructions.

- ```Dockerfile
  RUN <command>
  ```

  Executes `<command>` in a new layer on top of the current image and commits the results.

- ```Dockerfile
  COPY <src> <dest>
  ```

  Copies new files or directories from `<src>` and adds them to the filesystem of the container at the path `<dest>`.

- ```Dockerfile
  ADD <src> <dest>
  ```

  A more powerful version of `COPY` that also accepts remote file **URLs** as the `<src>`.

- ```Dockerfile
  ENV <key>=<value>
  ```

  Sets the environment variable `<key>` to the value `<value>`.

- ```Dockerfile
  VOLUME ["/data"]
  ```

  Creates a mount point with the specified name and marks it as holding externally mounted volumes from native host or
  other containers.

- ```Dockerfile
  CMD ["param1", "param2"]
  ```

  Provides defaults for an executing container.

- ```Dockerfile
  ENTRYPOINT command param1 param2
  ```

  Configures a container that will run as an executable.

  It is similar to `CMD` but the parameters we pass to the [`run`](#run) command are appended, not replaced.

  We can use both the `CMD` and `ENTRYPOINT` instructions (in **JSON** array format) if we want to define both a default
  command **AND** any default arguments that should be supplied to it when not passed through the [`run`](#run) command:

  ```Dockerfile
  # Dockerfile for an example `ubuntu-sleeper` image
  FROM Ubuntu

  ENTRYPOINT ["sleep"]

  CMD ["5"]
  ```

  ```sh
  # Running the image
  docker run ubuntu-sleeper # runs `sleep 5`
  docker run ubuntu-sleeper 10 # runs `sleep 10`
  ```

  We can override the entrypoint by passing the `--entrypoint` flag to the [`run`](#run) command:

  ```sh
  # Run the (imaginary) `sleep2.0` command in place of `sleep`
  docker run --entrypoint sleep2.0 ubuntu-sleeper 10
  ```

- ```Dockerfile
  WORKDIR /path/to/workdir
  ```

  Sets the working directory for any `RUN`, `CMD`, `ENTRYPOINT`, `COPY` and `ADD` instructions that follow it in the
  `Dockerfile`.

### Building an image
To build an image, we can follow the steps below:

1. Create a file named `Dockerfile`
2. Write the instruction for setting up the image (e.g. installing dependencies, what the entry point is, etc.)
3. Build the image
   ```sh
   docker build Dockerfile -t <tag-name>
   ```
4. (Optionally) Make the image available on Docker Hub
   ```sh
   docker push <tag-name>
   ```

## Layered architecture
A **Docker** image consists of several layers. Each instruction in the [`Dockerfile`](#dockerfile) corresponds to a
layer in the resulting image. The more instructions we have in the `Dockerfile`, the more layers there will be in the
resulting image, which will lead to bigger image size and complexity.

**Docker** uses [storage drivers](#storage-drivers) to enable this layered architecture. Some of the common storage
drivers available on **Linux** include `aufs`, `btrfs`, `zfs`, `overlay`, and `overlay2`. **Docker** will choose the
best storage driver automatically based on the operating system. The different drivers provide different performance and
stability characteristics.

All layers are cached. This is useful in case of crashes or when we add new commands to the `Dockerfile`, because all
layers already built will be reused.

When we build an image, **Docker** creates the layers as *read-only*. Then, when we use the [`run`](#run) command to
create a container out of the image, **Docker** creates a new writable layer (*read + write*) on top of the image layer.
This writeable layer is used to store data created by the container, such as log files written by the applications
running inside it, any temporary files generated by the container, or just any file modified by the user on that
container. However, this layer exists only as long as the container is alive. A container lives only as long as the
process inside it is alive. If the process inside it stops or crashes, the container exits. When that happens, this
writeable layer and all of the changes stored in it are also destroyed.

If we wish to persist the data, we can create a [volume](#volumes).

**Docker** uses a *copy-on-write* mechanism. If we change a file in the image layers, **Docker** first copies it to the
(*writeable*) container layer, and then writes the changes in that layer.

## Networks
Every container gets an **IP** address assigned to it by default. Only the **Docker** host can access the container
using that **IP**. If we want a container to be accessible outside of the host, we need to map a port inside the
container to a free port on the **Docker** host:

```sh
docker run -p 80:3000 mysql
```

When we install **Docker** it creates 3 networks automatically: `bridge`, `host`, and `none`.

`bridge` is a private internal network created by **Docker** on the host. All containers are attached to this network by
defaut and they get an internal **IP** address, usually in the range `127.17.*` series.

The containers can access each other using this internal **IP** if desired.

To associate the container with any other network we use the `--network` flag to the [`run`](#run) command:

```sh
docker run ubuntu --network=none # or --network=host
```

If we associate the container to the `host` network, we take out any network isolation between the host and the container.

A container run using the `--network=none` flag doesn't get attached to any network, and doesn't have access to the
external network or other containers. It runs in an isolated network.

**Docker** has a built-in **DNS** server that helps the containers to resolve each other using their names. It runs on
`127.0.0.11`.

Networking in **Docker** is implemented using *isolated network namespaces* and *virtual **Ethernet** pairs*.

### Create a custom internal network
```sh
docker network create --driver bridge --subnet 182.18.0.0/16 custom-isolated-network
```

### List all networks
```sh
docker network ls
```

### Linking to a container
Links allow containers to discover each other and securely transfer information about one container to another
container. When we set up a link, we create a conduit between a source container and a recipient container. The
recipient can then access select data about the source.

To create a link, we pass the `--link` flag to the [`run`](#run) command:

```sh
docker run -d --name=vote -p 5000:80 --link redis:redis voting-app
```

This creates an entry in the `/etc/hosts` file of the `vote` container, allowing it to connect to the `redis` container
by name.

The `--link` flag takes the form:

```sh
--link <name or id>:<alias>
```

where `name` or `id` identifies the container we're linking to and `alias` is an alias for the link name (the entry in
the `hosts` file). We can omit the alias if it matches the name.

This way of linking containers is *deprecated* in favor of [user-defined networks](#create-a-custom-internal-network).
One feature that user-defined networks do not support that we can do with `--link` is sharing environment variables
between containers. However, we can use other mechanisms, such as [volumes](#volumes), to share environment variables
between containers in a more controlled way.

## Storage drivers
**Docker** uses storage drivers to store image layers, and to store data in the writable layer of a container. The
container's writable layer does not persist after the container is deleted, but is suitable for storing ephemeral data
that is generated at runtime.

Storage drivers are optimized for space efficiency, but (depending on the storage driver) write speeds are lower than
native file system performance, especially for storage drivers that use a *copy-on-write* filesystem.

For *write-intensive* data, data that must persist beyond the container's lifespan, and data that must be shared between
containers, we should use *volumes*.

## Volumes
Volumes are the preferred mechanism for persisting data generated by and used by **Docker** containers. While [bind
mounts](#bind-mounts) are dependent on the directory structure and OS of the host machine, volumes are completely
managed by **Docker**.

In addition, volumes are often a better choice than persisting data in a container's writable layer, because a volume
does not increase the size of the containers using it, and the volume's contents exist outside the lifecycle of a given
container.

```sh
docker run -d \
  --name devtest \
  --mount type=volume,source=my-vol,target=/app \
  nginx:latest
```

Unlike a bind mount, we can create and manage volumes outside the scope of any container.

### Create a volume
```sh
docker volume create my-vol
```

### List volumes
```sh
docker volume ls
```

### Inspect a volume
```sh
docker volume inspect my-vol
```

### Remove a volume
```sh
docker volume rm my-vol
```

## Bind mounts
*Bind mounts* have been around since the early days of **Docker**. They have limited functionality compared to
[volumes](#volumes). When we use a bind mount, a file or directory on the *host machine* is mounted into a container.
The file or directory is referenced by its absolute path on the host machine. By contrast, when we use a volume, a new
directory is created within **Docker's** storage directory on the host machine (`/var/lib/docker/volumes`), and
**Docker** manages that directory's contents.

The file or directory does not need to exist on the **Docker** host already. It is created on demand if it does not yet
exist. Bind mounts are very performant, but they rely on the host machine's filesystem having a specific directory
structure available.

```sh
docker run -v /opt/datadir:/var/lib/mysql mysql
```

We can't use [Docker CLI commands](#cli-commands) to directly manage bind mounts.

## Docker compose
**Compose** is a tool for defining and running multi-container **Docker** applications. With **Compose**, we use a
**YAML** file (`docker-compose.yml`) to configure our application's services. Then, with a single command, we create and
start all the services from our configuration.

**Compose** works in all environments: production, staging, development, testing, as well as **CI** workflows. It also
has commands for managing the whole lifecycle of our application:

- Start, stop, and rebuild services
- View the status of running services
- Stream the log output of running services
- Run a one-off command on a service

Using **Compose** is essentially a three-step process:

1. Define the app's environment with a [`Dockerfile`](#dockerfile) so it can be reproduced anywhere
2. Define the services that make up the app in `docker-compose.yml` so they can be run together in an isolated
   environment
3. Run `docker compose up` to bring up the entire application stack

There are multiple versions of the `docker-compose.yml` configuration file.

Version 1 is deprecated.

```yml
# docker-compose.yml (version 1)
redis:
  image: redis
db:
  image: postgres:9.4
vote:
  build: ./vote
  image: voting-app
  ports:
    - 5000:80
  links:
    - redis
result:
  build: ./result
  image: result-app
  ports:
    - 5001:80
  links:
    - db
worker:
  build: ./worker
  image: worker
  links:
    - redis
    - db
```

**Compose** files using the version 2 syntax must indicate the version number at the root of the document. All services
must be declared under the `services` key. Named volumes can be declared under the `volumes` key, and networks can be
declared under the `networks` key. By default, every container joins an application-wide default network, and is
discoverable at a hostname that's the same as the service name. This means [links](#linking-to-a-container) are largely
unnecessary.

```yml
# docker-compose.yml (version 2)
version: 2.4
services:
  redis:
    image: redis
    networks:
      - back-end
  db:
    image: postgres:9.4
    networks:
      - back-end
  vote:
    build: ./vote
    image: voting-app
    ports:
      - 5000:80
    depends_on:
      - redis
    networks:
      - front-end
      - back-end
  result:
    build: ./result
    image: result-app
    ports:
      - 5001:80
    networks:
      - front-end
      - back-end
  worker:
    build: ./worker
    image: worker
    networks:
      - back-end
networks:
  front-end:
  back-end:
```

Version 3 comes with support for [**Docker Swarm**](https://docs.docker.com/engine/swarm/).

The latest and recommended version of the **Compose** file format is defined by the [**Compose
Specification**](https://github.com/compose-spec/compose-spec/blob/master/spec.md). This format merges the 2.x and 3.x
versions.
