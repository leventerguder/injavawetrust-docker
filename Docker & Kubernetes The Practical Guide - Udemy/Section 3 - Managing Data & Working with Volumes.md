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