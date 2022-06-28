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

