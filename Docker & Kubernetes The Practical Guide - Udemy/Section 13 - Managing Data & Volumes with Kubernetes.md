# Understanding "State"

State is data created and used by your application which must not be lost.

- User-generated data, user accounts , ...
    - Often stored in a database but could also bee files.
- Intermediate results derived by the app.
    - Often stored in memory, temporary database tables o files.

**Volumes!**

- Because we are still dealing with containers.
- Kubernetes runs our Containers.
- Kubernetes needs to be configured to add Volumes to our Containers.

# Kubernetes & Volumes

Kubernetes can mount Volumes into Containers.

- A broad variety of Volume types /drivers are supported.
    - Local Volumes
    - Cloud provider specific volumes
- Volume lifetime depends on the Pod lifetime.
    - Volumes survive Container restarts
    - Volumes are removed when Pods are destroyed.

**Kubernetes Volumes**

- Supports many different Drivers and Types.
- Volumes are not necessarily persistent.

**Docker Volumes**

- Basically no driver/type support
- Volumes persist until manually cleared.

