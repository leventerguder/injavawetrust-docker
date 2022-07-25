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

## Persistent Volumes

Volumes are destroyed when a Pod is removed.

- hostPath partially works around tha in "One Node" environments.
- Pod and node independent volumes are sometimes required. --> Persistent Volumes

## "Normal" Volumes vs Persistent Volumes

Volumes allow you to persist data

**"Normal" Volumes**

- Volume is attached to Pod and Pod lifecycle.
- Defined and created together with Pod.
- Repetitive and hard to administer on a global level.

**Persistent Volumes**

- Volume is a standalone Cluster resource (Not attached to a Pod)
- Created standalone, claimed via a PVC
- Can be defined once and used multiple times.
