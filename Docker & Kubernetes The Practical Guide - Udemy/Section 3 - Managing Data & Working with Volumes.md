# Data

- Application (code+environment)
    - Written & Provided by developer
    - Added to image and container in build phase
    - Fixed: Can't be changed once image is built.
    - Read-only, hence stored in images.
- Temporary App Data
    - Fetched/Produced in running container
    - Stored in memory or temporary files.
    - Dynamic and changing but cleared regularly.
    - Read + Write , temporary, hence stored in Containers
- Permanent App Data
    - Fetches/ Produced in running container
    - Stored in files or a database
    - Must not be lost if container stops/restarts
    - Read+write permanent, stored with Containers& Volumes

# Understanding Volumes

Volumes are folders on your host machine hard drive which are mounted into containers.

Volumes persist if a container shuts down. If a container (re-)starts and mount a volume, any data inside of that volume
is available in the container.

A container can write data into a volume and read data from it.

## Two Types of External Data Storages

- Volumes (Managed By Docker)
    - Anonymous Volumes
    - Named Volumes

Docker sets up a folder/path on your host machine, exact location is unknown to you.
Managed via docker volume commands.

A defined path in the container is mapped to the created volume/mount.
e.g /some-path on your hosting machine is mapped to /app/data

Great for data which should be persistent but which you don't need to edit directly.

- Bind Mounts (Managed by you)
    - You define a folder/path on your host machine.
    - Great for persistent, editable (by you) data. (e.g source code)

```
  docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback feedback-node:volumes
```

Anonymous volumes are removed automatically, when a container is removed.
This happens when you start/run a container with --rm option.

If you start a container without that, the anonymous volume would not be removed, even if you remove the container.

Still, if you then re-create and re-run the container , a new anonymous volume will be created. o even though the
anonymous volume wasn't removed automatically, it'll also not be helpful because a different anonymous volume is
attached the next time the container starts (i.e. you removed the old container and run a new one).

Now you just start piling up a bunch of unused anonymous volumes - you can clear them via docker volume rm VOL_NAME or
docker volume prune.

     docker run -d -p 3000:80 --rm --name feedback-app 
    -v feedback:/app/feedback 
    -v /Users/levent/Desktop/data-volumes-03-adj-node-code:/app 
    -v /app/node_modules feedback-node:volumes

## Volumes & Bind Mounts - Quick Overview

- docker run -v /app/data -> anonymous volume
- docker run -v data:/app/data -> named volume
- docker run -v /path/to/code:/app/code -> bind mount

**Anonymous Volume**

- Created specifically for a single container.
- Survives container shutdown/restart unless --rm is used.
- Can not be shared across containers.
- Since it's anonymous, it can't be re-used (even on same image)

**Named Volumes**

- Created in general - not tied to any specific container
- Survives container shutdown /restart-removal via Docker CLi
- Can be shared across containers
- Can be re-used for same container

**Bind Mounts**

- Location on host file system, not tied to any specific container.
- Survives container shutdown/restart-removal on host fs.
- Can be shared across containers
- Can be re-used for same container.

Volume ; A folder/file inside of Docker container which is connected to some folder outside of the container.

Volumes are managed by Docker, you dont necessarily know where the host folder is.

Anonymous Volumes are removed when a container, that was started with --rm is stopped.

Named Volumes survive container removal.

Bind Mount ; a path on your host machine , which you know and specified , that is mapped to some container-internal
path. You want to provide live data to container (no rebuilding needed).

    docker volume inspect feedback
    docker volume rm feedback
    docker volume prune

**Adding more to the .dockerignore File**

You can add more "to-be-ignored" files and folders to your .dockerignore file.

## Arguments & Environment Variables

Docker supports build-time arguments and runtime environment variables.

ARG

- Available inside of Dockerfile, not accessible in CMD or any application code.
- Set on image build docker build via --build-arg

ENV

- Available inside of Dockerfile & in application code.
- Set via ENV in Dockerfile or via --env on docker run.

  docker build -t feedback:env .
  docker run -d --rm -p 3000:8000 --env PORT=8000 --name feedback-app -v feedback:/app/feedback -v /app/node_modules -v
  /app/tem feedback-node:env

## Environment Variables & Security

One important note about environment variables and security: Depending on which kind of data you're storing in your
environment variables, you might not want to include the secure data directly in your Dockerfile.

Instead, go for a separate environment variables file which is then only used at runtime (i.e. when you run your
container with docker run).

Otherwise, the values are "baked into the image" and everyone can read these values via docker history <image>.

For some values, this might not matter but for credentials, private keys etc. you definitely want to avoid that!

If you use a separate file, the values are not part of the image since you point at that file when you run docker run.
But make sure you don't commit that separate file as part of your source control repository, if you're using source
control.

## Module Summary

Containers can read + write data. Volumes can help with data storage , Bind Mounts can help with direct container
interaction.

- Container can read + write data but written data is lost if the container is removed.
- Volumes are folders on the host machine , managed by Docker which are mounted into the Container.
- Named Volumes survive container removal and can therefore be used to store persistent data.
- Anonymous Volumes are attached to a container. They can be used to save (temporary) data inside the container.
- Binds Mounts are folders on the host machine whic are specified by the user and mounted into containers like Named
  Volumes.
- Build ARGuments and runtime ENVironment variables can be used to make images and container more dynamic/configurable.
