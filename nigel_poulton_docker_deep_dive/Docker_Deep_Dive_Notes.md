# Chapter 1 : Container from 30.000 feet

## The bad old days

Most applications run on servers. In the past we could only run one application per server.
Every time the business needed a new application, the IT department would buy a new server.
This resulted in over-powered servers operating as low as 5-10% of their potential capacity. A tragic waste of company
capital and environmental resources!

## Hello VMware!

This was a game changer. IT departments no longer needed to procure a brand-new oversized server every time the business
needed a new application. More often than not, they could run new apps on existing servers that were sitting around with
spare capacity.

## VMwarts

The fact that every VM requires its own dedicated operating system (OS) is a major flaw.
Every OS needs patching and monitoring.And in some cases, every OS requires a license. All of this results in wasted
time and resources.

VMs are slow to boot, and portability isn’t great — migrating and moving VM workloads between hypervisors and cloud
platforms is harder than it needs to be.

## Hello Containers!

A major difference is that containers do not require their own full-blown OS.
In fact, all containers on a single host share the host’s OS. This frees up huge amounts of system resources such as
CPU, RAM, and storage. It also reduces potential licensing costs and reduces the overhead of OS patching and other
maintenance.

Containers are also fast to start and ultra-portable.

## Linux Containers

Modern containers started in the Linux world and are the product of an immense amount of work from a wide variety of
people over a long period of time.

Some of the major technologies that enabled the massive growth of containers in recent years include; kernel namespaces,
control groups, union filesystems, and of course Docker.

## Hello Docker !

It’s enough to say that Docker was the magic that made Linux containers usable for mere mortals. Put another way,
Docker, Inc. made containers simple!

## Windows containers

Windows containers are available on the Windows desktop and Windows Server platforms.
The core Windows kernel technologies required to implement containers are collectively referred to as Windows
Containers.

## Windows containers vs Linux containers

It’s vital to understand that a running container shares the kernel of the host machine it is running on. This means
that a containerized Windows app will not run on a Linux-based Docker host, and vice-versa — Windows containers require
a Windows host, and Linux containers require a Linux host.

It is possible to run Linux containers on Windows machines. For example, Docker Desktop running on Windows has two modes
— “Windows containers” and “Linux containers”. Depending on your version of Docker Desktop, Linux container run either
inside a lightweight Hyper-V VM or using the Windows Subsystem for Linux (WSL). The WSL option is newer and the
strategic option for the future as it doesn’t require a Hyper-V VM and offers better performance and compatibility.

## What about Mac containers?

There is currently no such thing as Mac containers.
However, you can run Linux containers on your Mac using Docker Desktop. This works by seamlessly running your containers
inside of a lightweight Linux VM on your Mac.

## What about Kubernetes

Kubernetes is an open-source project out of Google that has quickly emerged as the de facto orchestrator of
containerized apps. That’s just a fancy way of saying Kubernetes is the most popular tool for deploying and managing
containerized apps.

Kubernetes uses Docker as its default container runtime — the low-level technology that pulls images and starts and
stops containers.

# Chapter 2: Docker

## Docker - The TLDR

Docker is software that runs on Linux and Windows. It creates, manages, and can even orchestrate containers.
Docker, Inc. is the company that created the technology and continues to create technologies and solutions that make it
easier to get the code on your laptop running in the cloud.

## The Docker Technology

When most people talk about Docker, they’re referring to the technology that runs containers. However, there are at
least three things to be aware of when referring to Docker as a technology:

- The runtime
- The daemon (a.k.a engine)
- The orchestrator

The runtime operates at the lowest level and is responsible for starting and stopping containers.
The low-level runtime is called "runc" and is the reference implementation of Open Containers Initiative (OCI)
runtime-spec.

The higher-level runtime is called conteinerd. 'containerd' does a lot more than 'runc'.
It manages the entire lifecycle of a container, including pulling images, creating network interfaces, and managing
low-level runc instances.

The Docker daemon (dockerd) sits above containerd and performs higher-level tasks such as; exposing the Docker remote
API, managing images , managing volumes, managing networks, and more...

## The Open Container Initiative (OCI)

The OCI is a governance council responsible for standardizing the low-level fundamental components of container
infrastructure.

https://opencontainers.org/

## Chapter summary

Docker, Inc. is a technology company out of San Francisco with an ambition to change the way we do software.
They were arguably the first-movers and instigators of the modern container revolution.

The Docker technology focuses on running and managing application containers. It runs on Linux and Windows,
can be installed almost anywhere, and is currently the most popular container runtime used by Kubernetes.

The Open Container Initiative (OCI) was instrumental in standardizing the container runtime format and container image
format.

# Chapter 3: Installing Docker

## Docker Desktop

Docker Desktop is a packaged product from Docker, Inc. It runs on 64-bit versions of Windows 10 and Mac, and it’s easy
to download and install.

Docker Desktop on Windows 10 can run native Windows containers as well as Linux containers. Docker Desktop on Mac can
only run Linux containers.

### Installing Docker Desktop on Mac

Docker Desktop for Mac is like Docker Desktop on Windows 10 — a packaged product from Docker, Inc with a simple
installer that gets you a single-engine installation of Docker that’s ideal for local development needs. You can also
enable a single-node Kubernetes cluster.

Docker Desktop on Mac doesn’t give you the Docker Engine running natively on the Mac OS Darwin kernel. Behind the
scenes, the Docker daemon is running inside a lightweight Linux VM that seamlessly exposes the daemon and API to your
Mac environment. This means you can open a terminal on your Mac and use the regular Docker commands.

Although this works seamlessly on your Mac, don’t forget that it’s Docker on Linux under the hood.
so it’s only going work with Linux-based Docker container.

docker version
Notice that the OS/Arch: for the Server component is showing as linux/amd64. This is because the daemon is running
inside of the Linux VM we mentioned earlier. The Client component is a native Mac application and runs directly on the
Mac OS Darwin kernel (OS/Arch: darwin/amd64).

# Chapter 4: The Big Picture

## The Ops Perspective

When you install Docker , you get two major components:

- the Docker client
- the Docker daemon

The daemon implements the runtime, API and everything else required to run Docker.
In a default Linux installation, the client talks to the daemon via a local IPC/Unix socket at /var/run/docker.sock.

### Images

It’s useful to think of a Docker image as an object that contains an OS filesystem, an application, and all application
dependencies.

In the Docker world, an image is effectively a stopped container. If you’re a developer, you can think of an image as a
class.

docker image ls

docker image pull ubuntu:latest
Run the docker image ls command again to see the image you just pulled.

docker images
It’s also worth noting that each image gets its own unique ID. When referencing images, you can refer to them using
either IDs or names. If you’re working with image ID’s, it’s usually enough to type the first few characters of the ID —
as long as it’s unique, Docker will know which image you mean.

### Containers

Now that we have an image pulled locally, we can use the docker container run command to launch a container from it.

docker container run -it ubuntu:latest /bin/bash
docker container run tells the Docker daemon to start a new container. The -it flags tell Docker to make the container
interactive and to attach the current shell to the container's terminal.

You can see all running containers on your system using the docker container ls command.

docker container ls

### Attaching to running containers

You can attach your shell to the terminal of a running container with the docker container exec command.

docker start hopeful_elion
docker container exec -it hopeful_elion bash
ps
The format of **docker container exec** is **docker container exec<options><container-name or container-id> <
/command/app>

Stop the container and kill it using the docker container stop and docker container rm commands. Remember to substitute
the names/IDs of your own containers.

docker container stop hopeful_elion
docker container rm hopeful_elion
Verify that the container was successfully deleted by running the docker container ls command with the -a flag. Adding
-a tells Docker to list all containers, even those in the stopped state.

docker container ls -a

## The Dev Perspective

Use the docker image build command to create a new image using the instructions in the Dockerfile.

docker image build -t test:latest .

docker container run -d
--name web1
--publish 8080:8080
test:latest

# Chapter 5 : The Docker Engine

## Docker Engine - The TLDR

The Docker engine is the core software that runs and manages containers.

The Docker engine is modular in design and built from many small specialised tools. Where possible, these are based on
open standards such as those maintained by the Open Container Initiative (OCI).

The major components that make up the Docker engine are; the Docker daemon, containerd, runc, and various plugins such
as networking and storage.

## Docker Engine - The Deep Dive

When Docker was first released, the Docker engine had two major components:

- The Docker daemon
- LXC

The Docker daemon was a monolithic binary. It contained all of the code for the Docker client, the Docker API, the
container runtime, image builds, and much more.

LXC provided the daemon with access to the fundamental building-blocks of containers that existed in the Linux kernel.
Things like namespaces and control groups. (cgroups)

### Getting rid of LXC

LXC is Linux-specific. This was a problem for a project that had aspirations of being multi-platform.

As a result, Docker. Inc. developed their own tool called **libcontainer** as a replacement for LXC. The goal of
**libcontainer** was to be a platform-agnostic tool that provided Docker with access to the fundamental container
building-blocks that exist in the host kernel.

### Getting rid of the monolithic Docker daemon

The monolithic nature of the Docker daemon became more and more ploblematic:

- It's hard to innovate on
- It got slower.
- It wasn't what the ecosystem wanted.

This work of breaking apart and re-factoring the Docker engine has seen all of the container execution and container
runtime code entirely removed from the daemon and refactored into small, specialized tools.

### The influence of the Open Container Initiative (OCI)

While Docker, Inc was breaking the daemon apart and refactoring code, the OCI was in the process of defining two
container-related specifications;

- Image spec
- Container runtime spec

The Docker engine implements the OCI specifications as closely as possible.

### runc

The **runc** is the reference implementation of the OCI container-runtime spec.
runc has a single purpose in life - create containers.

### containerd

All of the container execution logic was ripped out and refactored into a new tool called **containerd**.
Its sole purpose in life was to manage container lifecycle operations -start | stop | pause | rm ...

In the Docker engine stack , containerd sits between the daemon and runc at the OCI layer.

**containerd** was originally intended to be small, lightweight, and designed for a single task in life - container
lifecycle operations.However, over time it has branched out and taken on more functionality. Things like image pulls,
volumes and networks.

### Starting a new container

The following docker container run command will start a simple new container based on the alpine:latest image.

docker container run --name ctr1 -it alpine:latest sh
When you type commands like this into the Docker CLI , the Docker clients converts them into the appropriate API payload
and POSTs them to the API endpoint exposed by the Docker daemon.

One the daemon receives the command to create a new container, it makes a call to containerd.
The daemon communicates with container via a CRUD-style API over gRPC.

Despite its name, **containerd** cannot actually create containers. It uses **runc** to do that. It converts the
required Docker image into an OCI bundle and tells runc to use this to create a new container.

runc interfaces with the OS kernel to pull together all of the constructs necessary to create a container.

### One huge benefit of this model

Having all of the logic and code to start and manage containers removed from the daemon means that the entire container
runtime is decoupled from the Docker daemon.

In the old model, where all of container runtime logic was implemented in the daemon, starting and stopping the daemon
would kill all running containers on the host. This was a huge problem in production environments

Fortunately, this is no longer a problem.

### What's this shim all about ?

The shim is integral to the implementation of daemonless containers.

**containerd** uses **runc** to create new containers. In fact, it forks a new instance of runc for every container it
creates. However, once each container is created, the parent runc process exits. This means we can run hundreds of
containers without having to run hundreds of runc instances.

### How it’s implemented on Linux

The components we've discussed are implemented as separate binaries as follows:

- dockerd (The Docker daemon)
- docker-containerd(containerd)
- docker-container-shim(shim)
- docker-runc(runc)

### What's the point of the daemon

At the time of writing, some of the major functionality that still exists in the daemon includes; image
management, image builds, the REST API, authentication, security, core networking, and orchestration.

## Securing client and daemon communication

Docker implements a client-server model.

- The client component implements the CLI
- The server(daemon) component implements the functionality, including the public-facing REST API.

By default, network communication occur over an unsecured HTTP socket on port 2375/tcp

Docker lets you force the client and daemon to only accept network connections that are secured with TLS. This is
recommended for production environments, even if all traffic is traversing trusted internal networks.

## Chapter summary

The Docker engine is modular in design and based heavily on open-standards from the OCI.
The Docker daemon implements the Docker API which is currently a rich, versioned, HTTP API that has developed alongside
the rest of the Docker project.

Container execution is handled by **containerd.** You can think of it as a container supervisor that handles container
lifecycle operations. It is small and lightweight and can be used by other projects and third-party tools.

containerd needs to talk to an OCI-compliant container runtime to actually create containers. By default, Docker uses
runc as its default container runtime. runc is the de facto implementation of the OCI runtime-spec and expects to start
containers from OCI-compliant bundles.
containerd talks to runc and ensures Docker images are presented to runc as OCI-compliant bundles.

runc can be used as a standalone CLI tool to create containers. It’s based on code from libcontainer, and can also be
used by other projects and third-party tools.

There is still a lot of functionality implemented in the Docker daemon. More of this may be broken out over time.
Functionality currently still inside of the Docker daemon includes, but is not limited to; the Docker API, image
management, authentication, security features and core networking.

# Chapter 6 Images

## Docker images - The TLDR

A Docker image is a unit of packaging that contains everything required for an application to run. This includes;
application code , application dependencies, and OS constructs.

If you are a former VM admin, you can think of Docker images as similar to VM templates.
A VM template is like a stopped VM.
A Docker image is like a stopped container. If you are a developer you can think of them as similar to class.

You can get Docker images by pulling them from an image registry.

Images are made up multiple layers that are stacked on top of each other and represented as a single object.

## Docker images - The deep dive

### Images and containers

We use the **docker container run** and **docker service create** commands to start one or more containers from a single
image. Attempting to delete an image without stopping and destroying all containers using it will result in an error.

### Images are usually small

The whole purpose of a container is to run a single application or service. This means it only need to code and
dependencies of the app/service it is running - it does not need anything else.

### Pulling images

docker image pull redis:latest
...
docker image pull alpine:latest
...
docker image ls

### Image registries

We store images in centralised places called image registries. This makes it easy to share and access them.
The most common registry is Docker Hub (https://hub.docker.com)

docker info
.....
.....
Registry: https://index.docker.io/v1/
....

### Official and unofficial repositories

Docker Hub has the concept of official repositories and unofficial repositories.

Official repositories are the home to images that have been vetted and curated by Docker.
Unofficial repositories can be like the wild-west — you should not assume they are safe, well-documented or built
according to best practices.

### Image naming and tagging

The format for **docker image pull** when working with an image from an official repository is :

docker image pull <repository>:<tag>

docker image pull alpine:latest
docker image pull redis:latest
If you do not specify an image tag after the repository name, Docker will assume you are referring to the image tagged
as **latest**. If the repository doesn't have an image tagged as **latest** the command will fail.

Second, the latest tag doesn’t have any magical powers. Just because an image is tagged as latest does not guarantee it
is the most recent image in a repository.

Pulling images from an unofficial repository is essentially the same — you just need to prepend the repository name with
a Docker Hub username or organization name.

If you want to pull images from 3rd party registries (not Docker Hub), you need to prepend the repository name with the
DNS name of the registry.

docker image pull gcr.io/google-containers/git-sync:v3.1.5

### Images with multiple tags

A single image can have as many tags as you want.

Pull all of the images in a repository by adding the -a flag to the docker image pull command. Then run docker image ls
to look at the images pulled.

docker image pull -a nigelpoulton/tu-demo
docker image ls

nigelpoulton/tu-demo latest c610c6a38555 11 months ago 58.1MB
nigelpoulton/tu-demo v2 c610c6a38555 11 months ago 58.1MB
nigelpoulton/tu-demo v1 6ba12825d092 11 months ago 58.1MB
nigelpoulton/tu-demo v2-old d5e1e48cf932 2 years ago 104MB
nigelpoulton/tu-demo v1-old 6852022de69d 2 years ago 104MB
You will see that two of the IDs match. This is because two of the tags refer to the same image. Put another way, one of
the images has two tags.

Moral of the story, latest is an arbitrary tag and is not guaranteed to point to the newest image in a repository!

### Filtering the output of docker image ls

Docker provides the -- filter flag to filter the list of images returned by docker image ls.

docker image ls --filter dangling=true
A dangling image is an image that is no longer tagged, and appears in listings as < none >:< none >. A common way they
occur
is when building a new image giving it a tag that already exists. When this happens, Docker will build the new image,
notice that an existing image already has the same tag, remove the tag from the existing image and give it to the new
image.

You can delete all dangling images on a system with the docker image prune command. If you add the -a flag Docker
will also remove all unused images. (those not in use by any containers.)

- dangling : Accepts true or false , and returns only dangling images (true) , or non-dandling images (false).
- before : Requires an image name or ID as argument , and returns all images created before it.
- since : Same as above, but returns images created after the specified image.
- label : Filters images based on the presence of a label or label and value. The docker image ls command does not
  display labels in its output.

Here is an example ;

docker image ls --filter=reference="*:latest"
You can also use the --format flag to format output ;

docker image ls --format "{{.Size}}"
docker image ls --format "{{.Repository}}: {{.Tag}}: {{.Size}}"

### Searching Docker Hub from the CLI

The docker search command lets you search Docker Hub from the CLI. This has limited value as you can only pattern-match
against strings in the “NAME” field.

docker search leventerguder
...
docker search nigelpoulton
....
docker search alpine
You can use --filter "is-official=true" so that only official repos are displayed.

docker search alpine --filter "is-official=true"
By default, Docker will only display 25 lines of results. However, you can use the --limit flag to increase that to a
maximum of 100.

### Images and Layers

A Docker image is just a bunch of loosely-connected read only layers, with each layer comprising one or more files.

![](image-layers.png)

Docker takes care of stacking these layers and representing them as a single unified object.
All Docker images start with a base layer , and as changes are made and new content is added, new layers are added on
top.

It is important to understand that as additional layers are added, the image is always the combination of all layers
stacked in the order they were added.

### Sharing image layers

Multiple images can , and do, share layers. This leads to efficiencies in space and performance.
Docker is smart enough to recognize when it’s being asked to pull an image layer that it already has a local copy of.

### Pulling images by digest

Tags are mutable. This means it's possible to accidentally tag an image with the wrong tag. Sometimes, it’s even
possible to tag an image with the same tag as an existing, but different, image. This can cause problems!

Docker 1.10 introduced a content addressable storage model. As part of this model, all images get a cryptographic
content hash. You cannot change the content of an image and keep the old digest.

This means digests are immutable and provide a solution to the problem we just talked about.

Every time you pull an image, the docker image pull command includes the image’s digest as part of the information
returned.

    docker image ls --digests alpine
    docker image rm alpine:latest
    docker image pull alpine@sha256:686d8c9dfa6f3ccfc8230bc3178d23f84eeaf7e457f36f271ab1acc53015037c

### A little bit more about image hashes (digests)

Each images is identified by a crypto ID that is a hash of the config file. Each layer is identified by a crypto ID that
is a has of the layer content. We call these "content hashes".

This means that changing the content of the image, or any of its layers, will cause the associated crypto hashes to
change. As a result, images and layers are immutable, and we can easily identify any changes made to either.

Each layer also gets something called a distribution hash.

As well as providing a cryptographically verifiable way to verify image and layer integrity, it also avoids ID
collisions that could occur if image and layer IDs were randomly generated.

### Multi-architecture images

We’re using the term “architecture” to refer to CPU architecture such as x64 and ARM. We use the term “platform” to
refer to either the OS (Linux or Windows) or the combination of OS and architecture.

Docker and Docker Hub have a slick way of supporting multi-arch images.
Docker will pull the correct image for your platform and architecture.

To make this happen, the Registry API supports two important constructs:

- manifest lists
- manifests

The manifest list is exactly what is sounds like: a list of architectures supported by a particular image tag. Each
supported architecture then haas its own *manifest detailing the layers that make it up.

![](manifest-lists.png)

We do not have to tell Docker that we need the Linux or Windows version of image. We just run normal commands and let
Docker take care of getting the right image for the platform and architecture we are running.

    docker container run --rm golang go version
    ....
    go version go1.18.3 linux/arm64

All official images have manifest lists.
You can create your own builds for different platforms and architectures with docker buildx and then use docker manifest
create to create your own manifest lists.

    docker manifest inspect redis

### Deleting Images

Deleting an image will remove the image and all of its layers from your Docker host.

    docker image rm <image-name> | <image-id>

If the image you are trying to delete is in use by a running container you will not be able to delete it. Stop and
delete any containers before trying the delete operation again.

A handy shortcut for deleting all images on a Docker host is to run the docker image rm command and pass it a list of
all image IDs on the system by calling docker image ls with the -q flag.

    docker image rm $(docker image ls -q) -f

## Images - The commands

- docker image pull is the command to download images. We pull images from repositories inside of remote registries.
- docker image ls lists all of the images stored in your Docker host's local image cache.
- docker image inspect gives you all of the glorious details of an image.
- docker manifest inspect , allows you to inspect the manifest list of any image stored on Docker Hub
- docker buildx is a Docker CLI plugin that extends the Docker CLI to support multi-arch builds.
- docker image rm is the command to delete images.

# Chapter 7 : Containers

## Docker Containers - The TLDR

A container is the runtime instance of an image. In the same way that you can start a virtual machine (VM) from a
virtual machine template, you start one or more containers from a single image. The big difference between a VM and a
container is that containers are faster and more lightweight.

    docker container run -it ubuntu /bin/bash

The -it flags will connect your current terminal window to the container's shell.

The container will start, seize your terminal for 10 seconds, then exit. The following is a simple way to demonstrate
this on a Linux Docker host.

    docker container run -it alpine:latest sleep 10

You can manually stop a running container with the **docker container** stop command. You can then restart it with
**docker container start**
To get rid of container forever , **docker container rm**.

## Docker Containers - The deep dive

### Containers vs VMs

Containers and VMs both need a host to run.
Let’s assume a requirement where your business has a single physical server that needs to run 4 business applications.

Assuming the scenario of a single physical server that needs to run 4 business applications, we’d create 4 VMs, install
4 operating systems, and then install the 4 applications.

![](vm-scenario.png)

On top of the OS, we install a container engine such as Docker. The container engine then takes OS resources such as the
process tree, the filesystem, and the network stack, and carves them into isolated constructs called containers. Each
container looks smells and feels just like a real OS. Inside of each container we run an application.

If we assume the same scenario of a single physical server needing to run 4 business applications, we’d carve the OS
into 4 containers and run a single application inside each.

![](container-scenario.png)

At a high level, hypervisors perform hardware virtualization , they carve up physical hardware resources into virtual
versions called VMs. On the other hand, containers perform OS virtualization , they carve OS resources into virtual
versions called containers.

### The VM tax

The VM model carves low-level hardware resources into VMs. Each VM is a software construct containing virtual CPUs,
virtual RAM, virtual disks etc.As such, every VM needs its own OS to claim, initialize, and manage all of those virtual
resources. And sadly, every OS comes with its own set of baggage and overheads.

The container model has a single OS/kernel running on the host. It's possible to run tens or hundreds of containers on a
single host with every container sharing that single OS/kernel.
That means a single OS consuming CPU, RAM and storage. A single OS that needs licensing. A single OS that needs updating
and patching. And a single OS kernel presenting an attack surface. All in all, a single OS tax bill!

That might not seem a lot in our example of a single server running 4 business applications. But when you start talking
about hundreds or thousands of apps, it becomes a game-changer.

Another thing to consider is application start times. As a container isn’t a full-blown OS, it starts much faster than a
VM.

You can pack more applications onto less resources, start them faster, and pay less in licensing and admin costs, as
well as present less of an attack surface to the dark side.

Well, one thing that’s not so great about the container model is security. Out of the box, containers are less secure
and provide less workload isolation than VMs.

### Starting a simple container

The simplest way to start a container is with the **docker container run** command.

    docker container run -it ubuntu:latest /bin/bash

The **docker container run** tels Docker to run a new container. The -if flags make the container interactive and attach
it to your terminal.

Once the image was pulled , the daemon instructed containerd and runc to create and start the container.

### Container processes

When we started the Ubuntu container , this makes the Bash shell the one and only process running inside of the
container.
Killing the main process in the container will kill the container.

Press Ctrl-PQ to exit the container without terminating its main process.

    docker container ls

It's important to understand that this container is still running and you can re-attach your terminal to it with the
docker container exec command.

    docker container exec -it <container-id> bash

If you are following along with the examples, you should stop and delete the container with the following two commands.

    docker container stop <container-id>
    docker container rm <container-id>

### Container lifecycle

    docker conatainer run --name percy -it ubuntu:latest /bin/bash
    ...
    cd tmp
    echo "Sunderland is the greatest football team in the world" > newfile
    ...
    docker container stop percy

You can use the container's name or ID with the docker container stop command.

    docker container stop <container-id or container-name>
    docker container start percy
    docker container exec -it percy bash

Stopping a container does not destroy the container or the data inside of it.

- The data created in this example is stored on the Docker hosts local filesystem. If the Docker host fails, the data
  will be lost.
- Containers are designed to be immutable objects and it's not a good practice to write data to them.

For these reasons, Docker provides volumes that exist separately from the container, but can be mounted into the
container at runtime.

    docker container stop percy
    docker container rm percy
    docker container ls -a

To summarize the lifecycyle of a container ; you can stop , start, pause and restart a container as many times as you
want. It's not until you explicitly delete a container that you run a chance of losing its data.
Even then, if you are storing data outside the container in a volume, that data's going to persist even after the
container has gone.

### Stopping container gracefully

When you kill a running container with **docker container rm <container> -f** is killed without warning.
**docker container stop** command is far more polite.

The docker container stop sends a SIGTERM signal to the main application process inside the container (PID).
The docker container rm <container> -f goes straight to the SIGKILL.

### Self-healing containers with restart policies

Restart policies are applied per-container, and can be configured imperatively on the command line as part of docker
container run commands, or declaratively in YAML files for use with higher-level tools such as Docker Swarm, Docker
Compose and Kubernetes.

Restart policies ;

- always
- unless-stopped
- on-failed

The "always" policy is the simplest. It always restarts a stopped container unless it has been explicitly stopped, such
as via a docker container stop command.

      docker container run --name neversaydie -it --restart always alpine sh
      ...
      exit

An interesting feature of the --restart always policy is that if you stop a container with docker container stop and the
restart the Docker daemon, the container will be restarted.

If you restart the Docker daemon , the container will be automatically restarted when the daemon comes back up.
The main difference between the always and unless-stopped policies is that containers with the --restart unless-stopped
policy will not be restarted when the daemon restarts if they were in the Stopped (Exited) state.

      docker container run -d --name always-policy --restart always alpine sleep 1d
      docker container run -d --name unless-stopped-policy --restart unless-stopped alpine sleep 1d
      ...
      docker container ls

Once Docker has restarted , you can check the status of the containers.

      docker container ls -a

Notice that the "always-policy" container has been restarted, bt the "unless-stopped" container has not.
The on-failure policy will restart a container if it exits with a non-zero exit code. It will also restart containers
when the Docker daemon restarts, even containers that were in the stopped state.

## Web Server Example

Now let’s take a look at a Linux-based web server example.

    docker container run -d --name webserver -p 80:8080 nigelpoulton/pluralsight-docker-ci

This time we give it the -d flag instead of -it. -d stands for daemon mode , and tells the container to run in the
background.
You can't use -d and -it flags in the same command.

The -p flag maps port 80 on the Docker host to port 8080 inside the container. This means that that traffic hitting the
Docker host on port 80 will be directed to pot 800 inside of the container.

## Inspecting containers

When building a Docker image , you can embed an instruction that lists the default app for any containers that use the
image.
YOu can see this for any image by running a **docker image inspect**.

    docker image inspect nigelpoulton/pluralsight-docker-ci 

## Tidying up

This should never be performed on production systems or systems running important containers.

Run the following command from the shell of your Docker host to delete all containers.

    docker container rm $(docker container ls -aq) -f 
    docker image rm $(docker image ls -q) -f

## Containers - The commands

**docker container run** is the command used to start new containers.

    docker container run -it ubuntu /bin/bash

Ctrl+PQ will detach your shell from the terminal of a container and leave the container running(UP) in the background.

**docker container ls** lists all containers in the running (UP) state. If you add the -a flag you will also see
containers in the sopped (Exited) state.

**docker container exec runs** a new process inside of a running container.It's ueful for attaching the shell of your
Docker hosts to a terminal insiede of a running container.

    docker container exec -it <container-name or container-id> bash

**docker container stop** will stop a running container and put it in the Exited (O) state. It does this by issuing a
SIGTERM to the process with PID 1 inside of the container.

**docker container start** will restart a stopped (Exited) container. You can give docker container start the name or ID
of a container.

**docker container rm** will delete a stopped container. You can specify containers by name or ID. It is recommended
that you stop a container with the docker container stop command before deleting it with docker container rm.

**docker container inspect** will show you detailed configuration and runtime information about a container.

# Chapter 8 - Containerizing an app

The process of taking an application and configuring it to run as a container is called “containerizing".

## Containerizing an app - The TLDR

Containers all about making apps simple to build, ship, and run.
The process of containerizing an app looks like this:

- Start with your application code and dependencies
- Create a Dockerfile that describes your app, its dependencies, and how to run it.
- Feed the Dockerfile into the docker image build command.
- Push the new image to a registry(optional)
- Run container from the image.

![](containerizing-app.png)

## Containerizing an app - The deep dive

### Containerize a single-container app

**Getting the application code**

    git clone https://github.com/nigelpoulton/psweb.git
    ...    
    cd psweb
    ls -l

**Inspecting the Dockerfile**

A Docker file is the starting point for creating a container image - it describes an application and tells Docker how to
build it into an image.

The directory containing the application and dependencies is referred to as the build context. It's a common practice to
keep your Dockerfile in the root directory of the build context.It's also important that Dockerfile starts with a
capital "D" and is all one word.

    cat Dockerfile

    FROM alpine
    LABEL maintainer="nigelpoulton@hotmail.com"
    RUN apk app --update nodejs npm curl
    COPY . /src
    WORKDIR /src
    RUN npm install
    EXPOSE 8080
    ENTRYPOINT ["node","./app.js"]

All Dockerfiles start with the FROM instruction.This will be the base layer of the image, and the rest of the app will
be added on top as additional layers.
This particular application is a Linux app, so it’s important that the FROM instruction refers to a Linux-based image.
If you’re containerizing a Windows application, you’ll need to specify the appropriate Windows base image - such as
mcr.microsoft.com/dotnet/core/aspnet.

The RUN instruction uses the Alpine apk package manager to install nodejs and npm into the image. It creates layer
directly above the Alpine base layer, and installs the packages in this layer.

The COPY . /src instruction creates another new layer and copies in the application and dependency files from the build
context.

Next, the Dockerfile uses the WORKDIR instruction to set the working directory inside the image filesystem for the rest
of the instruction in the file. The instruction does not create a new image layer.

Then the RUN npm install instruction creates a new layer and uses npm to install application dependencies listed in the
package.json file in the build context.

The application exposes a web service on TCP port 8080, so the Dockerfile documents this with the EXPOSE 8080
instruction.
This is added as image metadata and not an image layer.

Finally, the ENTRYPOINT instruction is used to set the main application that the image (container) should run. This is
also added as metadata and not an image layer.

![](dockerfile-layers.png)

**Containerize the app/build the image**

The following command will build a new image called web:latest. The period (.) at the end of command tells Docker to use
the shell's current working directory as the build context.

    pwd
    .../psweb
    ...
    docker image build -t weeb:latest .
    ...
    docker image ls

**Pushing images**

In order to push an image to Docker Hub,you need to login with your Docker ID. You also need to tag the image
appropriately.

    docker login
    ..

Docker is opinionated, so by default it pushes images to Docker Hub. You can push to other registries, but you have to
explicitly set the registry URL as part of the docker image push command.

The previous docker image ls output shows the image is tagged as web:latest. This translates to a repository called web
and an image tagged as latest. As a result, docker image push will try and push the image to a repository called web on
Docker Hub.However, I don’t have access to the web repository, all of my images live in the nigelpoulton second-level
namespace.

    docker image tag web:latest <your-account>/web:latest

The format of the command is docker image tag <current-tag> <new-tag> and it adds an additional tag, it does not
overwrite the original.

You can't push images to repos in my Docker namespace, you will have to tag the image to use your own.

    docker image push <you-account>/web:latest
    ...

**Run the app**

    docker container run -d --name c1 -p 80:8080 web:latest
    docker container 

http://localhost:80/

**Looking a bit closer**

The docker image build command parses the Dockerfile one-line-at-a-time starting from the top.

All non-comment lines are Instructions and take the format INSTRUCTION argument. Instruction names are not case
sensitive, but it's normal practice to write them in UPPERCASE.

Some instructions create new layers, whereas others just add metadata to the image config file.

Examples of instructions that create new layers are FROM,RUN, and COPY. Examples that create metadata include EXPOSE,
WORKDIR, ENV, ENTRYPOINT. The basic premise is this - if an instruction is adding content such as files and programs to
image, it will create a new layer. If it is adding instructions on how to build the image and run the application, it
will create metadata.

You can view the instructions that were used to build the image with **docker image history** command.

    docker image history web:latest
    docker image inspect web:latest

## Moving to production with Multi-stage Builds

When it comes to Docker images, big is bad.
Big means slow. Big means hard to work with. And big means more potential vulnerabilities and possibly a bigger attack
surface.

For these reasons, Docker images should be small. The aim of the game is to only ship production images with the stuff
needed to run your app in production.

Multi-stage builds are all about optimizing builds without adding complexity. And they deliver on the promise.

Multi-stage builds have a single Dockerfile containing multiple FROM instructions. Each FROM instruction is a new build
stage that can easily COPY artefacts from previous stages.

    FROM node:latest AS storefront
    WORKDIR /usr/src/atsea/app/react-app
    COPY react-app .
    RUN npm install
    RUN npm run build
    
    FROM maven:latest AS appserver
    WORKDIR /usr/src/atsea
    COPY pom.xml .
    RUN mvn -B -f pom.xml -s /usr/share/maven/ref/settings-docker.xml dependency:resolve
    COPY . .
    RUN mvn -B -s /usr/share/maven/ref/settings-docker.xml package -DskipTests
    
    FROM java:8-jdk-alpine
    RUN adduser -Dh /home/gordon gordon
    WORKDIR /static
    COPY --from=storefront /usr/src/atsea/app/react-app/build/ .
    WORKDIR /app
    COPY --from=appserver /usr/src/atsea/target/AtSea-0.0.1-SNAPSHOT.jar .
    ENTRYPOINT ["java", "-jar", "/app/AtSea-0.0.1-SNAPSHOT.jar"]
    CMD ["--spring.profiles.active=postgres"]

The first thing to note is that the Dockerfile has three FROM instructions. Each of these constitutes a distinct build
stage. Internally, they are numbered from top starting at 0.

    Stage 0 is called storefront
    Stage 1 is called appserver
    Stage 2 is called production

An important thing to note, is that COPY --from instructions are used to only copy production-related application code
from the images built by the previous stages. They do not copy build artefacts that are not needed for production.

    docker image build -t multi:stage .

## A few best practices

### Leverage the build cache

The best way to see the impact of the cache is to build a new image on a clean Docker host, then repeat the same build
immediately after. The first build will pull images and take time building layers. The second build will complete almost
instantaneously. This is because artefacts from the first build, such as layers are cached and leveraged by later
builds.

The docker image build process iterates through a Dockerfile one-line-at-a time starting from the top. For each
instruction, Docker looks to see if it already has an image layer for that instruction in its cache.
If it does, this is a cache hit, and it uses that layer. If it doesn't, this is a cache miss, and it builds a new layer
from the instruction.

### Squash the image

Squashing an image isn't really a best practice as it has pros and cons.

Squashing can be good in situations where images are starting to have a lot of layers and this isn’t ideal. An example
might be when creating a new base image that you want to build other images from in the future — this base is much
better as a single-layer image.

On the negative side, squashed images do not share image layers. This can result in storage inefficiencies and larger
push and pull operations.

Add the --squash flag to the docker image build command if you want to create a squashed image.

The squashed image will also need to send every byte to Docker Hub on a docker image push command, whereas the
non-squashed image only needs to send unique layers.

![](squashed vs non-squashed images.png)

### Use no-install-recommends

If you are building Linux images and using the apt package manager, you should use the no-install-recommends flag with
the apt-get install command. This makes sure that apt only installs main dependencies and not recommended or suggested
packages. This can greatly reduce the number of unwanted packages that are downloaded into your images.

## Containerizing an app - The commands

**docker image build** is the command that reads a Dockerfile and containerizes an application. The -t flag tags the
image, and the -f flag lets you specify the name and location of the Dockerfile. With the -f flag, it is possible to use
a Docker file with an arbitrary name and in an arbitrary location.

The **FROM** instruction in a Dockerfile specifies the base image for the new image you will build. It is usually the
first instruction in a Dockerfile and a best-practice is to use images from official repos on this line.

The **RUN** instruction in a Dockerfile allows you to run commands inside the image. Each RUN instruction
creates a single new layer.

The **COPY** instruction in a Dockerfile adds files into the image as a new layer. It is common to use the COPY
instruction to copy your application code into an image.

The **EXPOSE** instruction in a Docker file documents the network port that the application uses.

The **ENTRYPOINT** instruction in a Dockerfile sets the default application to run when the image is started as a
container.

Other Dockerfile instructions include LABEL, ENV , ONBUILD, HEALTHCHECK, CMD and more...

# Chapter 9 - Deploying Apps with Docker Compose

In this chapter we will focus on Docker Compose, which deploys and manages multi-container applications on Docker nodes
running in single-engine mode. In a later chapter, we will focus on Docker Stacks.
Stacks deploy and manage multi-container apps on Docker nodes running in swarm mode.

## Deploying apps with Compose - The TLDR

Deploying and managing lots of small microservices like these can be hard. This is where Docker Compose comes in to
play.

Instead of gluing each microservice together with scripts and long docker commands , Docker Compose lets you describe an
entire app in a single declarative configuration file, and deploy it with a single command.

## Deploying apps wih Compose - The Deep Dive

### Compose Background

In the beginning was Fig. Fig was a powerful tool, created by company called Orchard, and it was the best way to manage
multi-container Docker apps. Docker, Inc acquired Orchard and re-branded FIg as Docker Compose.

Compose is still an external Python binary that you have to install on a Docker host. You define multi-container apps in
a YAML file, pass the YAML file to the **docker compose** command line, and Compose deploys it via the Docker API.

### Installing Compose

Docker Compose is installed as part of Docker Desktop for XXX. If you have Docker Desktop , you have Docker Compose.

    docker-compose --version

### Compose files

Compose uses YAML files to define multi-service applications. YAML is a subset of JSON.
The default name for Compose YAML file is docker-compose.yml. However, you can use the -f flag to specifiy custom
filenames.

```
    version: "3.8"
    services:
      web-fe:
        build: .
        command: python app.py
        ports:
          - target: 5000
            published: 5000
        networks:
          - counter-net
        volumes:
          - type: volume
            source: counter-vol
            target: /code
      redis:
        image: "redis:alpine"
        networks:
          counter-net:
    networks:
      counter-net:
    volumes:
      counter-vol:

```

The **version** key is mandatory, nad it's always the first line at the root of the file. This defines the version of
the Compose file format. You should normally use the latest version.
It's important to note that the **version** key does not define the version of Dockor Compose or the Docker Engine.

The **services** keys is where you define the different microservices. This example defines two services; wef-fe and
redis. Compose will deploy each of these services as its own container.

The **networks** key tells Docker to create new networks. By default, Compose will create a bridge networks. These are
single-host networks that can only connect containers on the same Docker host.

The **volumes** keys is where you tell Docker to create new volumes.

**Our specific Compose file**

The services section has two second-level keys.

- web-fe
- redis

This means Compose will deploy two containers, one will have web-fe in its name and the other will have redis.

**build** : This tells Docker to build new image using the instruction in the Dockerfile in the current directory (.).

**command** : python app.py , this tells Docker to run a Python app called app.py as the main app in the container. The
app.py file must exist in the image, and the image must contain Python.

**ports** : Tells Docker to map port 5000 inside the container (target) to port 5000 on the host(published). This means
that traffic sent to the Docker host on port 5000 will be directed to port 5000 on the container.

**networks** :  Tell Docker which network to attach the service's container to. The network should already exist, or be
defined in the networks top-level key.

**volumes**: Tells Docker to mount the counter-vol volume (source) to /code (target) inside the container. The counter
volume needs to already exist or be defined in the volumes top-level key at the bottom of the file.

As both services will be deployed onto the same counter-network , they will be able to resolve each other by name.
This is important as the application is configured to communicate with the redis service by name.

### Deploying an app with Compose

Clone the Git repo locally.

    git clone https://github.com/nigelpoulton/counter-app.git
    cd counter-app
    ls
    ......    
    Dockerfile		app.py			requirements.txt
    README.md		docker-compose.yml

Let's use Compose to bring the app up.

    docker-compose up&

The **docker-compose up** is the most common way to bring up a Compose app. It builds or pulls all required images,
creates all required networks and volumes, and starts all required containers.

By default, docker-compose up expects the name of the Compose file to docker-compose.yml. If your compose file has a
different name, you need to specify it with the -f flag.

    docker-compose -f prod-compose-file.yml

It's also common to use the -d flag to bring the app up in the background.

    docker-compose up -d

Our example brought the app up in the foreground (we didn’t use the -d flag), but we used the & to give us the terminal
window back. This forces Compose to output all messages to the terminal window.

The following network and volume listings shows the counter-app_counter-net and counter-app_counter-vol

    docker network ls

    docker volume ls

### Managing an App with Compose

As the application is already up, let's see how to bring it down. To do this ;

    docker-compose down

It is important to note that the counter-vol volume was not deleted. This is because volumes are intended to be
long-term persistent data stores.

Also, any images that were built or pulled as part of the docker-compose up operation will still be present on the
system. This means future deployments of the app will be faster.

Use the following command to bring the app up again, but this time in the background.

    docker-compose up -d

Show the current state of the app with the docker-compose ps command

    docker-compose ps

Use docker-compose top to list the processes running inside of each service(container).

    docker-compose top

Use the docker-compose stop command to stop the app without deleting its resources.

    docker-compose stop

You can delete a stopped Compose app with docker-compose rm. This will delete the containers and networks the app is
using, but it will not delete volumes or images.

Restart the app with the docker-compose restart command.

    docker-compose restart

Use the docker-compose down command to stop and delete the app with a single command.

    docker-compose down

The app is now deleted. Only its images, volumes, and source code remain.

## Deploying apps with Compose - The Commands

**docker-compose up** is the command to deploy a Compose app. It expects the Compose file to be called
docker-compose.yml or docker-compose.yaml , but you can specficy a custom filename with the -f flag.

**docker-compose stop** will stop all of the containers in a Compose app without deleting them from the system. The app
can be easily restarted with docker-compose restart.

**docker-compose rm** will delete a stopped Compose app. It will delete containers and networks, but it will not delete
volumes and images.

**docker-compose restart** will restart a Compose app that has been stopped with docker-compose stop. If you have made
changes to your Compose app since stopping it, these changes will not appear in the restarted app.
You will need to re-deploy the app to get the changes.

**docker-compose ps** will list each container in the Compose app. It shows current state, the command each one is
running, and network ports.

**docker-compose down** will stop and delete a running Compose app. It deletes containers and networks, but not vlumes
and images.

# Chapter 10 - Docker Swarm

## Docker Swarm - The TLDR

Docker Swarm is two main things;

- An enterprise-grade secure cluster of Docker hosts.
- An engine for orchestrating microservices apps.

On the clustering front, Swarm groups one or more Docker nodes and lets you manage them as a cluster. Out-of-the-box you
get an encrypted distributed cluster store, encrypted networks, mutual TLS, secure cluster join tokens, and a PKI that
makes managing and rotating certificates a breeze.

On the orchestration front, Swarm exposes a rich API that allows you to deploy and manage complex microservices apps
with ease. You can define your apps in declarative manifest files and deploy them to the Swarm with native Docker
commands. You can even perform rolling updates, rollbacks, and scaling operations.

Docker Swarm competes directly with Kubernetes - they both orchestrate containerized applications.

## Docker Swarm - The Deep Dive

### Swarm Primer

On the clustering front, a swarm consists of one or more Docker nodes. These can be physical servers, VMs, or cloud
instances.

Nodes are configured as managers or workers.

The configuration and state of the swarm is held in a distributed etcd database located on all managers.
It's kept in memory and is extremely up-to-date.

Swarm uses TLS to encrypt communications, authenticate nodes, and authorize roles.

On the application orchestration front, the atomic unit of scheduling on a swarm is the service. This is a new object in
the API, introduced along with swarm, and higher level construct that wraps some advanced feature around containers.
These include scaling, rolling updates, and simple rollbacks.

### Build a secure Swarm cluster

The nodes can be virtual machines, physical servers, cloud instances, or Raspberry Pi systems. The only requirements are
that they have Docker installed and can communicate over a reliable network.

Docker Desktop for Mac and Windows only supports a single Docker node. You can initialize a single-node swarm and follow
along with most of the examples.

**Initializing a new swarm**

Running docker swarm init on a Docker host in single-engine mode will switch that node into swarm mode, create a new
swarm and make the node the first manager of the swarm.

    docker swarm init

This tells Docker to initialize a new swarm and make this node the first manager.

The default port that swarm mode operates on is 2377. This is customizable but it's convention to use 2377/tcp for
secured HTTPS client-to-swarm-connections.

List the nodes in the swarm.

    docker node ls

### Swarm manager high availability (HA)

Swarm managers have native support for high availability. This means one or more can fail , and the survivors will keep
the swarm running. This means that although you have multiple managers, only one of them is active at any given moment.
This active manager is called the "leader" , and the leader is the only one that will ever issue live commands against
the swarm. If a follower manager(passive) receives commands for the swarm , it proxies them across to the leader.

![](high-availability.png)

This is Raft terminology, because swarm uses an implementation of the Raft consensus algorithm to maintain a consistent
cluster state across multiple highly available managers.

https://raft.github.io/

- Deploy an odd number of managers
- Don't deploy too many managers (3 or 5 is recommended)

Having an odd number of managers reduces the chances of split-brain conditions. For example, if you had 4 managers and
the network partitioned, you could be left with two managers on each side of the partition. This is known as a split
brain - each side knows there used to be 4 but can not only see 2.

### Built-in Swarm security

Swarm clusters have a ton of built-in security that-s configured out-of-the box with sensible defaults.

### Locking a Swarm

Despite all of this built-in native security, restarting an older manager or restoring an old backup has the potential
to compromise the cluster.

To prevent situations like these, Docker allows you to lock a swarm with the Autolock feature. This forces restarted
managers to present the cluster unlock key before being admitted back into the cluster.

It's possible to apply a lock directly to a new swarm by passing the --autolock flag to the docker swarm init command.
However, we've already built a swarm, so we'll lock our existing swarm with the docker swarm update command.

    docker swarm update --autolock=true

Be sure to keep the unlock key in a secure place. You can always check your current swarm unlock key with the docker
swarm unlock-key command.

Restart one of your manager nodes to see if it automatically re-joins the cluster.

Try and list the nodes in the swarm.

    docker node ls
    ....
    Error response from daemon: Swarm is encrypted and needs to be unlocked before it can be used. Please use "docker swarm
    unlock" to unlock it.

Although the Docker service has restarted on the manager, it has not been allowed to re-join the swarm.

Use the docker swarm unlock command to unlock the swarm for the restarted manager.

    docker swarm unlock

### Swarm services

Services let us specify most of the familiar container options, such as name, port mappings, attaching to networks and
images. But they add important cloud native features desired state and automatic reconciliation. For example, swarm
services allow us to declaratively define a desired state for an application that we can apply to swarm and let the
swarm take care of deploying it and managing it.

You can create services in one of two ways:

- Imperatively on the command line with docker service create.
- Declaratively with a stack file

### Viewing and Inspecting Services

You can use the docker service ls command to see a list of all services running on a swarm.

### Replicated vs Global Services

The default replication mode of a service is replicated. This deploys a desired number of replicas and distributes them
as evenly as possible across the cluster.

The other mode is global, which runs a single replica on every node in the swarm.

To deploy a global service you need to pass the --mode global flag to the docker service create command.

### Scaling a Service

Another powerful feature of service is the ability to easily scale them up and down.

    docker service scale web-fe=10
    docker service ps web-fe

### Removing a service

The following docker service rm command will delete the service deployed earlier.

    docker service rm web-fe
    docker service ls

Be careful using the docker service rm command as it deletes all service replicas without asking for confirmation.

### Rolling updates

We’re going to deploy a new service. But before we do that, we’re going to create a new overlay network for the service.
This isn’t necessary, but I want you to see how it is done and how to attach the service to it.

    docker network create -d overlay uber-net

Run a docker network ls to verify that the network created properly and is visible on the Docker host.

    docker network ls

    docker service create --name uber-svc \
    --network uber-net \
    -p 80:80 --replicas 12 \
    nigelpoulton/tu-demo:v1

Let’s also assume that you’ve been tasked with pushing the updated image to the swarm in a staged manner — 2 replicas at
a time with a 20 seconds delay between each. You can use the following docker service update command to accomplish this.

    docker service update \
    --image nigelpoulton/tu-demo:v2 \
    --update-parallelism 2 \
    --update-delay 20s uber-svc

### Backing up Swarm

Backing up a swarm will backup the control plane objects required to recover the swarm in the event of a catastrophic
failure of corruption. Recovering a swarm from a backup is an extremely rare scenario. However, business critical
environments should always be prepared for worst-case scenarios.

### Docker Swarm - The Commands

**docker swarm init** is the command to create a new swarm. The node that you run the command on becomes the first
manager
and is switched to run in swarm mode.

**docker swarm join-token** reveals the commands and tokens needed to join workers and managers to existing swarms. To
expose the command to join a new manager, use the docker swarm join-token manager command. To get the command to join a
worker, use the docker swarm join-token worker command.

**docker node ls** lists all nodes in the swarm including which are managers and which is the leader.

**docker service create** is the command to create a new service.

**docker service ls** lists running services in the swarm and gives basic info on the state of the service and any
replicas it’s running.

**docker service ps <service>** gives more detailed information about individual service replicas.

**docker service inspect** gives very detailed information on a service. It accepts the --pretty flag to
limit the information returned to the most important information.

**docker service scale** lets you scale the number of replicas in a service up and down.

**docker service update** lets you update many of the properties of a running service.

**docker service logs** lets you view the logs of a service.

**docker service rm** is the command to delete a service from the swarm. Use it with caution as it deletes
all service replicas without asking for confirmation.

# Chapter 11 Docker Networking

## Docker Networking - The TLDR

Docker run applications inside of containers, and applications need to communicate over lots of different networks.
This means Docker needs strong networking capabilities.

Docker networking is based on an open-source pluggable architecture called the Container Network Model (CNM).
**libnetwork** is Docker's real-world implementation of the CNM, and it provides all of Docker's core networking
capabilities.

To create a smooth out-of-the box experience, Docker ships with a set of native drivers that deal with the most common
networking requirements. These include single-host bridge networks, multi-host overlays, and options for plugging into
existing VLANs.

## Docker Networking - The Deep Dive

### The theory

At the highest level, Docker networking comprises three major components:

- The Container Network Model (CNM)
- The libnetwork
- Drivers

The CNM is the design specification.It outlines the fundamental building blocks of a Docker network.

The libnetwork is real-world implementation of the CNM, and is used by Docker. It's written in Go, implements the core
components outlined in the CNNM.

Drivers extend the model by implementing specific netwok topologies such as VXLAN overlay networks.

**The Container Network Model (CNM)**

The design guide for Docker networking is the CNM. It outlines the fundamental building blocks of a Docker network.

It defines three major building blocks:

- Sandboxes
- Endpoints
- Networks

A sandbox is an isolated network stack. It includes ; Ethernet interfaces , ports, routing tables and DNS config.

Endpoints are virtual network interfaces. Like normal network interfaces , they're responsible for making connections.

Networks are a software implementation of an switch (802.1d bridge). As such, they group together and isolate a
collection of endpoints that need to communicate.

![img.png](container-network-model.png)

**libnetwork**

The CNM is the design doc, and libnetwork is the canonical implementation. It's open-source, written in GO,
cross-platform , and used by Docker.

All of the core Docker networking code lives in libnetwork.

**Drivers**

IF libnetwork implements the control plane and management plane functions, then drivers implement the date plane.
For example, connectivity and isolation are all handled by drivers.

Docker ships with several built-in drivers, known as native drivers or local drivers. On Linux they include; bridge,
overlay, and macvlan. On Windows they include; nat, overlay, transparent, and l2bridge.

3rd-parties can also write Docker network drivers known as remote drivers or plugins.

### Single-host bridge networks

The simplest type of Docker network is the single-host bridge network.

- Single-host tells us it only exists on a single docker host and can only connect containers that are on the same host.
- Bridge tells us that it's an implementation of an 802.1d bridge.

Docker on Linux creates single-host bridge networks with the built-in bridge driver, whereas Docker on Windows creates
them using the built-in nat driver.

The following listing shows the output of a docker network ls command on newly installed Linux and Windows Docker hosts

    docker network ls
    docker network inspect bridge

Docker networks built with the bridge driver on Linux hosts are based on the battle-hardened linux bridge technology
that has existed in the Linux kernel for nearly 20 years. This means they’re high performance and extremely stable.

The default "bridge" network, on all Linux based Docker hosts, maps to an underlying Linux bridge in the kernel called "
docker0".

    docker network inspect bridge | grep bridge.name
    ...
    "com.docker.network.bridge.name": "docker0"

![img.png](docker-linux-network.png)

Let's use the **docker network create** command to create a new single-host bridge network called  "localnet".

    docker network create -d bridge localnet
    docker network ls

Let’s create a new container and attach it to the new localnet bridge network.

    docker container run -d --name c1 \
    --network localnet \
    alpine sleep 1d

This container will now be on the localnet network. You can confirm this with a docker network inspect.

    docker network inspect localnet --format '{{json .Containers}}'

If we add another new container to the same network, it should be able to ping the "c1" container by name. This is
because all new containers are automatically registered with the embedded Docker DNS service, enabling them to resolve
the names of all other containers on the same network.

Create a new interactive container called "c2" and putit on the same localnet network as "c1"/

    docker container run -it --name c2 \
    --network localnet \
    alpine sh

    ...
    ping c1

Port mappings let you map a container to a port on the Docker host. Any traffic hitting the Docker host on the
configured port will be directed to the container.

Run a web server container and map port 80 on the container to port 5000 on the Docker host.

    docker container run -d --name web \
    --network localnet \
    --publish 5000:80 \
    nginx

### Multi-host overlay networks

Overlay networks are multi-host. They allow a single network to span multiple hosts so that containers on different
hosts can communicate directly. They’re ideal for container-to-container communication, including container-only
applications, and they scale well.

Docker provides a native driver for overlay networks. This makes creating them as simple as adding the --d overlay flag
to the docker network create command.

### Connecting to existing networks

The following command will create a new MACVLAN network called “macvlan100” that will connect containers to VLAN 100.

    docker network create -d macvlan \
    --subnet=10.0.0.0/24 \
    --ip-range=10.0.0.0/25 \
    --gateway=10.0.0.1 \
    -o parent=eth0.100 \
    macvlan100

MACVLAN uses standard Linux sub-interfaces, and you have to tag them with the ID of the VLAN they will connect to. In
this example we’re connecting to VLAN 100, so we tag the sub-interface with .100 (etho.100).

The macvlan100 network is ready for containers, so let’s deploy one with the following command.

    docker container run -d --name mactainer1 \
    --network macvlan100 \
    alpine sleep 1d

### Service Discovery

As well as core networking, libnetwork also provides some important network services.

Service discovery allows all containers and Swarm services to locate each other by name. The only requirement is that
they be on the same network.

Under the hood, this leverages Docker's embedded DNS server and the DNS resolver in each container.

Every Swarm service and standalone container started with the --name flag will register its name and IP with the Docker
DNS service. This means all containers and service replicas cna use the Docker DNS service to find each other.

However, service discovery is network-scoped. This means that name resolution only works for containers and Services on
the same network. If two containers are on different networks, they will not be able to resolve each other.

It's possible to configure Swarm services and standalone containers with customized DNS options.
For example, the --dns flag lets you specify a list of custom DNS servers to use in case the embedded Docker DNS server
cannot resolve a query.

    docker container run -it --name con1 \
    --dns=8.8.8.8 \
    --dns-search=nigelpoulton.com \
    alpine sh

### Ingress load balancing

Swarm supports two publishing modes that make services accessible outside of the cluster:

- Ingress mode (default)
- Host mode

Services published via ingress mode can be accessed from any node in the Swarm - even nodes not running a service
replica.

Services published via host mode can only be accessed by hitting nodes running service replicas.

Ingress mode is the default. This means any time you publish a service with -p or --publish it will default to ingress
mode. To publish a service in host mode you need to use the long format of the --publish flag and add mode=host. Let’s
see an example using host mode.

Behind the scenes, ingress mode uses a layer 4 routing mesh called the Service Mesh or the Swarm Mode Service Mesh.

## Docker Networking - The Commands

**docker network ls** : Lists all networks on the local Docker host.

**docker network create** : Creates new Docker networks. By default, it creates them with the nat driver
on Windows and the bridge driver on Linux. You can specify the driver (type of network) with the -d flag. docker network
create -d overlay overnet will create a new overlay network called overnet with the native Docker overlay driver.

**docker network inspect** : Provides detailed configuration information about a Docker network.

**docker network prune** : Deletes all unused networks on a Docker host.

**docker network rm**: Deletes specific networks on a Docker host.

## Chapter Summary

The Container Network Model (CNM) is the master design document for Docker networking and defines the three major
constructs that are used to build Docker networks — sandboxes, endpoints, and networks.

libnetwork is the open-source library, written in Go, that implements the CNM. It’s used by Docker and is where all of
the core Docker networking code lives. It also provides Docker’s network control plane and management plane.

Drivers extend the Docker network stack (libnetwork) by adding code to implement specific network types, such as bridge
networks and overlay networks. Docker ships with several built-in drivers, but you can also use 3rd-party drivers.

Single-host bridge networks are the most basic type of Docker network and are suitable for local development and very
small applications. They do not scale, and they require port mappings if you want to publish your services outside of
the network. Docker on Linux implements bridge networks using the built-in bridge driver, whereas Docker on Windows
implements them using the built-in nat driver.

Overlay networks are all the rage and are excellent container-only multi-host networks. We’ll talk about them in-depth
in the next chapter.

The macvlan driver (transparent on Windows) allows you to connect containers to existing physical networks and VLANs.
They make containers first-class citizens by giving them their own MAC and IP addresses. Unfortunately, they require
promiscuous mode on the host NIC, meaning they won’t work in the public cloud.

Docker also uses libnetwork to implement basic service discovery, as well as a service mesh for container-based load
balancing of ingress traffic.

# Chapter 12 - Docker Overlay networking

Overlay networks are at the beating heart of many cloud-native microservices apps.

## Docker Overlay networking - The TLDR

In the real world, it's vital that containers can communicate with each other reliably and securely, even when they're
on different hosts that are on different networks. This is where overlay networking comes in to play.
It allows you to create a flat, secure, layer-2 network, spanning multiple hosts. Containers connect to this and can
communicate directly.

Docker offers native overlay networking that is simple to configure and secure by default.

Behind the scenes, it's built on top of libnetwork and drivers. libnetwork is the canonical implementation of the
Container Network Model (CNM) and drivers are pluggable components that implement different networking technologies and
topologies. Docker offers native drivers, including the overlay driver.

## Docker Overlay networking - The Deep dive

    docker network create -d overlay uber-net
    docker network ls

### Attach a service to the overlay network

Now that you have an overlay network, let's create a new Docker service and attach it to the network.

    docker service create --name test \
    --network uber-net \
    --replicas 2 \
    ubuntu sleep infinity

Verify the operation with a docker service ps command.

    docker service ps test

### The theory of how it all works

**VXLAN Primer**

Docker overlay networking uses VXLAN tunnels to create virtual Layer 2 overlay networks.
At the highest level, VXLANs let you create a virtual Layer2 network on top of existing Layer3 infrastructure.

The beauty of VXLAN is that it’s an encapsulation technology that existing routers and network infrastructure just see
as regular IP/UDP packets and handle without issue.
Each end of the VXLAN tunnel is terminated by a VXLAN Tunnel Endpoint (VTEP).

## Docker overlay networking - The commands

**docker network create** is the command that we use to create a new container network. The -d flag lets you specify the
driver to use, and the most common driver is the overlay driver. You can also specify remote drivers from 3rd parties.
For overlay networks, the control plane is encrypted by default. Just add the -o encrypted flag to encrypt the data
plane (performance overheads may be incurred).

**docker network ls** lists all of the container networks visible to a Docker host. Docker hosts running in swarm mode
only
see overlay networks if they are hosting containers attached to that particular network. This keeps network-related
gossip to a minimum.

**docker network inspect** shows you detailed information about a particular container network. This includes scope,
driver, IPv4 and IPv4 info, subnet configuration, IP addresses of connected containers, VXLAN network ID, and encryption
state.

**docker network rm** deletes a network

# Volumes and persistent data

Stateful applications that persist data are becoming more and more important in the world of cloud-native and
microservices applications.

## Volumes and persistent data - The TLDR

To deal with non-persistent data, every Docker container gets is own non-persistent storage. This is automatically
created for every container and is tighttly coupled to the lifecycle of the container.

As a result , deleting the container will delete the storage and any data on it.

To deal with persistent data, a container needs to store it in a volume. Volumes are separate objects that have their
lifecycle decoupled from containers.
This means you can create and manage volumes independently, and they are not tied to the lifecycle of container.

## Volumes and persistent data - The Deep Dive

### Containers and non-persistent data

Containers are designed to be immutable.

The writable container layer exists in the filesystem of the Docker host, and you’ll hear it called various names.
These include local storage, ephemeral storage, and graphdriver storage.

This thin writable layer is an integral part of a container and enables all read/write operations. If you, or an
application, update files or add new files, they’ll be written to this layer. However, it’s tightly coupled to the
container’s lifecycle - it gets created when the container is created and it gets deleted when the container is deleted.

If your containers don’t create persistent data, this thin writable layer of local storage will be fine and you’re good
to go.

This writable layer of local storage is managed on every Docker host by a storage driver (not to be confused with a
volume driver).

### Containers and persistent data

Volumes are the recommended way to persist data in containers.

- Volumes are independent objects that are not tied to the lifecycle of a containers
- Volumes can be mapped to specialized external storage systems.
- Volumes enable multiple containers on different Docker hosts to access and share the same data

Docker volume existing outside of the container as a separate object. It is mounted into the container's filesystem ,
and any data written to the dicertory will be stored on the volume and will exist after the container is deleted.

![](high-level-view-volumes-containers.png)

**Creating and managing Docuker volumes**

    docker volume create myvol

By default, Docker creates new volumes with the built-in local driver. As the name suggests, volumes created with the
local driver are only available to containers on the same node as the volume. You can use the -d flag to specify a
different driver.

    docker volume ls
    docker volume inspect myvol

Notice that the Driver and Scope are both local. This means the volume was created with the local driver and is only
available to containers on this Docker host.

There are 2 ways to delete a Docker volume :

    docker volume prune
    docker volume rm

docker volume prune will delete all volumes that are not mounted into a container or service replica, so use with
caution! docker volume rm lets you specify exactly which volumes you want to delete. Neither command will delete a
volume that is in use by a container or service replica.

### Demonstrating Volumes with Containers and Services

    docker container run -dit --name voltainer \
    --mount source=bizvol,target=/vol \
    alpine

The command uses the --mount flag to mount a volume called "bizvol" into the container at either /vol.
The command completes successfully despite the fact there is no volume on the system called bizvol. This raises an
interesting point:

- If you specify an existing volume , Docker will use the existing volume
- If you specify a volume that doesn't exist, Docker will create it for you.

  docker volume ls

Although containers and volumes have separate lifecycle , you can not delete a volume that is in use by a container.

    docker volume rm bizvol

Lets exec onto the container and write some data to it.

    docker container exec -it voltainer sh
    echo "I promise to write a review of the book on Amazon" > /vol/file1
    exit

    docker container rm voltainer -f

Even though the container is deleted, the volume still exists :

    docker volume ls

It's even possible to mount the bizvol volume into a new service or container.

    docker container run -it --name hell-cat \
    --mount source=bizvol,target=/vol \
    alpine sh

    cat /vol/file1

### Sharing storage across cluster nodes

Integrating external storage systems with Docker makes it possible to share volumes between cluster nodes. These
external systems can be cloud storage services or enterprise storage systems in your on-premises data centers.

A major concern with any configuration that shares a single volume among multiple containers is data corruption.

## Volumes and persistent data - The Commands

**docker volume create** is the command we use tho create new volumes. By default, volumes are created with the local
driver, but you can use the -d flag to specify a different driver.

**docker volume ls** will list all volumes on the local Docker host.

**docker volume inspect** shows detailed volume information. Use this command to see many interesting volume properties.

**docker volume prune** will delete all volumes that are not in use by a container or service replica. Use with caution!

**docker volume rm** deletes specific volumes that are not in use.

**docker plugin install** with install new volume plugins from Docker Hub.

**docker plugin ls** list all plugins installed on a Docker host.

## Chapter Summary

There are two main types of data: persistent and non-persistent data. Persistent data is data that you need to keep,
non-persistent is data that you don’t need to keep. By default, all containers get a layer of writable non-persistent
storage that lives and dies with the container — we call this local storage and it’s ideal for non-persistent data.
However, if your containers create data that you need to keep, you should store the data in a Docker volume.

Docker volumes are first-class citizens in the Docker API and managed independently of containers with their own docker
volume sub-command. This means that deleting a container will not delete the volumes it was using.

Third party volume plugins can provide Docker access to specialised external storage systems. They’re installed from
Docker Hub with the docker plugin install command and are referenced at volume creation time with the -d command flag.

Volumes are the recommended way to work with persistent data in a Docker environment.

# Chapter 14  Deploying apps with Docker Stacks

Deploying and managing cloud-native microservices applications comprising lots of small integrated services at scale is
hard. Fortunately, Docker Stacks are here to help. They simplify application management by providing; desired state,
rolling updates, simple, scaling operations, health checks, and more! All wrapped in a nice declarative model.

## Deploying apps with Docker Stacks - The TLDR

Fortunately, stacks are here to help!. They let you define complex multi-service apps in a single declarative file. They
also provide a simple way to deploy the app and manage its entire lifecycle — initial deployment > health checks >
scaling > updates > rollbacks and more!

Define the desired state of your app in a Compose file, then deploy and manage it with the docker stack command. That’s
it.

The Compose file includes the entire stack of microservices that make up the app. It also includes all of the volumes,
networks, secrets, and other infrastructure the app needs. The docker stack deploy command is used to deploy the entire
app from the single file.

To accomplish all of this, stacks build on top of Docker Swarm, meaning you get all of the security and advanced
features that come with Swarm.

In a nutshell, Docker is great for application development and testing. Docker Stacks are great for scale and
production.

## Deploying apps with Docker Stacks - The Deep Dive

Architecturally speaking, stacks are at the top of the Docker application hierarchy. They build on top of services,
which in turn build on top of containers.

### Overview of the sample app

    git clone https://github.com/dockersamples/atsea-sample-shop-app.git
    ...
    cat docker-stack.yml

At the highest level, it defines 4 top-level keys.

- version:
- services:
- networks:
- secrets:

The **version** indicates version of the Compose file format. This has to be 3.0 or higher to work with stacks.
The **services** is where you define the stack of services that make up the app.

The **networks** list the required networks and, the **secret** define the secrets the app uses.

It's important to understand that the stack file captures and defines many of the requirements of the entire
application.
As such, it's self-documenting and a great tool for bridging the gap between dev and ops.

### Looking closer at the stack file

Stack files are very similar to Compose files.

**Networks**

One of the first things Docker does when deploying an app from a stack file is create any required networks list under
the networks key. If the networks don't already exist, Docker creates them.

    networks:
        front-tier:
        back-tier:
        payment:
          driver: overlay
          driver_opts:
            encrypted: 'yes'

The stack file describes three networks; front-tier, back-tier, and payment. By default, they’ll all be created as
overlay networks by the overlay driver. But the payment network is special — it requires an encrypted data plane.

To encrypt the data plane, you have two choices:

- Pass the -o encrypted flag to the docker network create command.
- Specify encrypted : 'yes' under driver_opts in the stack file.

**Secrets**

    secrets:
      postgres_password:
        external: true
      staging_token:
        external: true
      revprox_key:
        external: true
      revprox_cert:
        external: true  

Notice that all four are defined as external. This means that they must already exist before the stack can be
deployed.

**Services**

Services are where most of the action happens.
Each service is a JSON collection (dictionary) that contains a bunch of keys.

**The reverse_proxy service**

    reverse_proxy:
      image: dockersamples/atseasampleshopapp_reverse_proxy
      ports:
        - "80:80"
        - "443:443"
      secrets:
        - source: revprox_cert
        target: revprox_cert
        - source: revprox_key
        target: revprox_key
      networks:
        - front-tier

The **image** key is the only mandatory key in the service object. As the name suggests, it defines the Docker image
that will be used to build the replicas for the service.

Docker is opinionated, so unless you specify otherwise, the image will be pulled from Docker Hub. You can specify images
from 3rd-party registries by prepending the image name with the DNS name of the registry’s API endpoint such as gcr.io
for Google’s container registry.

One difference between Docker Stacks and Docker Compose is that stacks do not support builds. This means all images have
to be built prior to deploying the stack.

By default, all ports are mapped using ingress mode. This means they’ll be mapped and accessible from every node in the
Swarm — even nodes not running a replica.

The secrets key defines two secrets — revprox_cert and revprox_key. These secrets must already exist on the swarm and
must also be defined in the top-level secrets section of the stack file.

The networks key ensures that all replicas for the service will be attached to the front-tier network.

**The database service**

The database service also defines; an image, a network, and a secret. As well as those, it introduces environment
variables and placement constraints.

    database:
      image: dockersamples/atsea_db
      environment:
        POSTGRES_USER: gordonuser
        POSTGRES_DB_PASSWORD_FILE: /run/secrets/postgres_password
        POSTGRES_DB: atsea
      networks:
        - back-tier
      secrets:
        - postgres_password
      deploy:
        placement:
          constraints:
            - 'node.role == worker'

The environment key lets you inject environment variables into service replicas at runtime. This service uses three
environment variables to define a dataase user, the location of database password , and the name of tha database.

**The appserver service**

The appserver service uses an image, attaches to three networks, and mounts a secret. It also introduces several
additional features under the deploy key.

    appserver:
      image: dockersamples/atsea_app
      networks:
        - front-tier
        - back-tier
        - payment
      deploy:
        replicas: 2
        update_config:
          parallelism: 2
          failure_action: rollback
        placement:
          constraints:
            - 'node.role == worker'
        restart_policy:
          condition: on-failure
          delay: 5s
          max_attempts: 3
          window: 120s
      secrets:
        - postgres_password

First up,services.appserver.deploy.replicas = 2 will set the desired number of replicas for the service to 2. If omitted
, the default value is 1.

services.appserver.deploy.update_config tells Docker how to act when updating the service. For this service, Docker will
update two replicas at-a-time (parallelism) and will perform a ‘rollback’ if it detects the update is failing.

The services.appserver.deploy.restart-policy object tells Swarm how to restart replicas (containers) if and when they
fail. The policy for this service will restart a replica if it stops with a non-zero exit code (condition: on-failure).
It will try to restart the failed replica 3 times, and wait up to 120 seconds to decide if the restart worked. It will
wait 5 seconds between each of the three restart attempts.

    restart_policy:
      condition: on-failure
      delay: 5s
      max_attempts: 3
      window: 120s

**The payment_gateway service**

The payment_gateway service specifies an image, mounts a secret, attaches to a network, defines a partial deployment
strategy, and then imposes a couple of placement constraints.

    payment_gateway:
      image: dockersamples/atseasampleshopapp_payment_gateway
      secrets:
        - source: staging_token
          target: payment_token
      networks:
        - payment
      deploy:
        update_config:
          failure_action: rollback
        placement:
          constraints:
            - 'node.role == worker'
            - 'node.labels.pcidss == yes'

Node labels are custom-defined labels added to swarm nodes with the docker node update command.
In this example, the payment_gateway service performs operations that require it to run on a swarm node that has been
hardened to PCI DSS standards. To enable this, you can apply a custom node label to any swarm node meeting these
requirements.

## Deploying apps with Docker Stacks - The Commands

**docker stack deploy** is the command for deploying and updating stacks of services defined in a stack file. (usually
called docker-stack.yml)

**docker stack ls** lists all stacks on the Swarm, including how many services they have.

**docker stack ps** gives detailed information about a deployed stack. It accepts the name of the stack as its main
argument, lists which node each replica is running on, and shows desired state and current state.

**docker stack rm** deletes a stack from the Swarm. It does not ask for confirmation before deleting the stack.

## Chapter Summary

Stacks are the native Docker solution for deploying and managing cloud-native microservices applications with multiple
services. They are baked into the Docker engine, and offer a simple declarative interface for deploying and managing the
entire lifecycle of an application.l

You start with application code and a set of infrastructure requirements - things like networks, ports, volumes and
secrets.You containerize the application and group together all of the app services and infrastructure requirements into
a single declarative stack file. You set the number of replicas, as well as rolling update and restart policies. Y YOu
then take the file and deploy the application from it using the docker stack deploy command.

Future updates to the deployed app should be done declaratively by checking the stack file out of source control,
updating it, re-deploying the app, and checking the stack file back into source control.

# Chapter 15 Security in Docker

## Security in Docker - The TLDR

Security is all about layers. Generally speaking, the more layers of security the more secure something is. Well...
Docker offers a lot of security layers.

![](docker-security.png)

Docker on Linux leverages most of the common Linux security and workload isolation technologies. These include
namespaces, control groups(cgroups), capabilities, mandatory access control (MAC) systems, and seccomp.

Docker Swarm Mode is secure by default. You get all of the following with zero configuration required; cryptographic
node IDs, mutual authentication, automatic CA configuration, automatic certificate rotation, encrypted cluster store,
encrypted networks, and more.

Image security scanning analyses images, detects known vulnerabilities, and provides detailed reports.

Docker secrets are a way to securely share sensitive data and are first-class objects in Docker. They’re stored in the
encrypted cluster store, encrypted in-flight when delivered to containers, stored in in-memory filesystems when in use,
and operate a least-privilege model.

## Security in Docker - The deep dive

When Docker decided to bake security into the platform, it decided to make it simple and easy.
As a result, most of the security technologies offered by the Docker platform are simple to use. They also ship with
sensible defaults — meaning you get a fairly secure platform at zero effort.

### Linux Security Technologies

**Namespaces**

Kernel namespaces are at the very heart of containers. They slice up an operating system (OS) so that it looks and feels
like multiple isolated operating systems. This lets us do really cool things like run multiple web servers on the same
OS without having port conflicts.

A high-level example of two web server applications running on a single host and both using port 443. Each web server
app is running inside of its own network namespace.

![](namespaces.png)

Working directly with namespaces is hard. Fortunately, Docker hides this complexity and manages all of the namespaces
required to build a useful container.

Docker on Linux currently utilizes the following kernel namespaces:

- Process ID (pid)
- Network (net)
- Filesystem/mount (mnt)
- Inter-process Communication (ipc)
- User(user)
- UTS(uts)

Docker containers are an organized collection of namespaces. This means that you get all of this OS isolation for free
with every container.

Every container has its own pid, net, mnt,ipc, uts, and potentially user namespace.

**Process ID namespace:** Docker uses the pid namespace to provide isolated process trees for each container. This means
every container gets its own PID 1. PID namespaces also mean that one container cannot see or access to the process tree
of other containers. Nor can it see or access the process tree of the host it’s running on.

**Network namespace:** Docker uses the net namespace to provide each container its own isolated network stack. This
stack
includes; interfaces, IP addresses, port ranges, and routing tables. For example, every container gets its own eth0
interface with its own unique IP and range of ports.

**Mount namespace:** Every container gets its own unique isolated root (/) filesystem. This means every container can
have
its own /etc, /var, /dev and other important filesystem constructs. Processes inside of a container cannot access the
mount namespace of the Linux host or other containers — they can only see and access their own isolated filesystem.

**Inter-process Communication namespace:** Docker uses the ipc namespace for shared memory access within a container. It
also isolates the container from shared memory outside of the container.

**User namespace:** Docker lets you use user namespaces to map users inside of a container to different users on the
Linux
host. A common example is mapping a container’s root user to a non-root user on the Linux host.

**UTS namespace:** Docker uses the uts namespace to provide each container with its own hostname.

**Control Groups**

If namespaces are about isolation, control groups (cgroups) are about setting limits.

**Capabilities**
It’s a bad idea to run containers as root — root is all-powerful and therefore very dangerous. But, it can be
challenging running containers as unprivileged non-root users. For example, on most Linux systems, non-root users tend
to be so powerless they’re practically useless. What’s needed, is a technology that lets us pick and choose which root
powers a container needs in order to run.

Under the hood, the Linux root user is a combination of a long list of capabilities. Some of these capabilities include:

- CAP_CHOWN: lets you change file ownership
- CAP_NET_BIND_SERVICE: lets you bind a socket to low numbered network ports
- CAP_SETUID: lets you elevate the privilege level of a process
- CAP_SYS_BOOT: lets you reboot the system.
- ...

This is an excellent example of implementing least privilege — you get a container running with only the capabilities
required.

**Mandatory Access Control Systems**

Docker works with major Linux MAC technologies such as AppArmor and SELinux.
Depending on your Linux distribution, Docker applies a default AppArmor profile to all new containers. According to the
Docker documentation, this default profile is “moderately protective while providing wide application compatibility”.

**seccomp**

Docker uses seccomp, in filter mode, to limit the syscalls a container can make to the host’s kernel.
As per the Docker security philosophy, all new containers get a default seccomp profile configured with sensible
defaults. This is intended to provide moderate security without impacting application compatibility.

**Final thoughts on the Linux security technologies**

Docker supports most of the important Linux security technologies and ships with sensible defaults that add security but
aren’t too restrictive.

![](linux-security.png)

### Docker platform security technologies

**Security in Swarm Mode**

Docker Swarm allows you to cluster multiple Docker hosts and deploy applications declaratively. Every Swarm is comprised
of managers and workers that can be Linux or Windows. Managers host the control plane of the cluster and are responsible
for configuring the cluster and dispatching work tasks. Workers are the nodes that run your application code as
containers.

Swarm mode includes many security features that are enabled out-of-the-box with sensible defaults.

- Cryptographic node IDs
- TLS for mutual authentication
- Secure join tokens
- CA configuration with automatic certificate rotation
- Encrypted cluster store (config DB)
- Encrypted networks

**Swarm join tokens**

The only thing that is needed to join new managers and workers to an existing swarm is the relevant join token. For this
reason, it’s vital that you keep your join-tokens safe. Do not post them on public GitHub repos or even internal source
code repos that are not restricted.

Every swarm maintains two distinct join tokens:

- One for joining new managers
- One for joining new workers.

It’s worth understanding the format of the Swarm join token. Every join token is comprised of 4 distinct fields
separated by dashes (-):

PREFIX - VERSION - SWARM ID - TOKEN

The prefix is always SWMTKN. This allows you to pattern-match against it and prevent people from accidentally posting it
publicly. The VERSION field indicates the version of the swarm. The Swarm ID field is a hash of the swarm’s certificate.
The TOKEN field is the part that determines whether it can join nodes as managers or workers.

**The cluster store**

The cluster store is the brains of a swarm and is where cluster config and state are stored. It’s also critical to other
Docker technologies such as overlay networking and Secrets. This is why swarm mode is required for so many advanced and
security related Docker features. The moral of the story... if you’re not running in swarm mode, there’ll be a bunch of
Docker technologies and security features you won’t be able to use.

The store is currently based on the popular etcd distributed database and is automatically configured to replicate
itself to all managers in the swarm. It is also encrypted by default.

**Detecting vulnerabilities with image security scanning**

Image scanning is your primary weapon against vulnerabilities and security holes in your images.

Image scanners work by inspecting images and searching for packages that have known vulnerabilities. Once you know about
these, you can update the packages and dependencies to versions with fixes.

**Signing and verifying images with Docker Content Trust**

Docker Content Trust (DCT) makes it simple and easy to verify the integrity and the publisher of images that you
download and run. This is especially important when pulling images over untrusted networks such as the internet.

You can force a Docker host to always sign and verify image push and pull operations by exporting the DOCKER_-
CONTENT_TRUST environment variable with a value of 1. In the real world, you’ll want to make this a more permanent
feature of Docker hosts.

    export DOCKER_CONTENT_TRUST=1

Once DCT is enabled, you’ll no longer be able to pull and work with unsigned images.

## Docker Secrets

Behind the scenes, secrets are encrypted at rest, encrypted in-flight, mounted in containers to in-memory filesystems,
and operate under a least-privilege model where they are only made available to services that have been explicitly
granted access to them. It’s quite a comprehensive end-to-end solution, and it even has its own docker secret
sub-command.

You can create and manage secrets with the docker secret sub-command, and you can attach them to services by specifying
the --secret flag to the docker service create command.

## Chapter Summary

Docker can be configured to be extremely secure. It supports all of the major Linux security technologies, including;
kernel namespaces, cgroups, capabilities, MAC, and seccomp. It ships with sensible defaults for all of these, but you
can customize them and even disable them.

Over and above the general Linux security technologies, Docker includes an extensive set of its own security
technologies. Swarm Mode is built on TLS and is extremely simple to configure and customize. Image scanning can perform
binary-level scans of images and provide detailed reports of known vulnerabilities. Docker Content Trust lets you sign
and verify content, and Docker Secrets allow you to securely share sensitive data with containers and Swarm services.