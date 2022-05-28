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

    docker container run -d \
    --name web1 \
    --publish 8080:8080 \
    test:latest