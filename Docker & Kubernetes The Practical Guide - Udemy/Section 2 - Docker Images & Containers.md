# Images vs Containers

Images

- Templates/Blueprints for containers
- Contains code + required tools/runtimes

Containers

- The running "unit of software"
- Multiple containers can be created based on one image

Images are blueprints for containers which then are running instances with read and write access.

Container , an isolated unit of software which is based on an image. A running instance of that image.

Multiple containers can be based on the same image but they are totally isolated from each other.
This concept allows multiple containers to be based on the same image without interfering with each other.

Containers are separated from each other and have no shared data or state by default.

Every instruction in an image creates a cacheable layer. Layers help with image re-building and sharing.