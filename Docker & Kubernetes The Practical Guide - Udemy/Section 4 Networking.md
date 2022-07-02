# Container & Networks

## Creating Container Network

    docker run --network my_network

Within a Docker network, all containers can communicate with each other and IPs are automatically resolved.

    docker network create favorites-net
    docker run -d --name mongodb --network favorites-net mongo
    docker run --name favorites --network favorites-net  --rm -p 3000:3000 favorites

By default , your containers can reach out to the web and send requests.

host.docker.internal, the special identifier is resolved to your local IP address.

Containers communicate with other containers , by manually finding the IP addresses or by using a network.

Containers can communicate with each other if they are in the same network. You can use the container names as
addresses.

## Docker Network Drivers

Docker Networks actually support different kinds of "Drivers" which influence the behavior of the Network.

The default driver is the "bridge" driver - it provides the behavior shown in this module (i.e. Containers can find each
other by name if they are in the same Network)

The driver can be set when a Network is created, simply by adding the --driver option.

    docker network create --driver bridge my-net

Of course, if you want to use the "bridge" driver, you can simply omit the entire option since "bridge" is the default
anyways.

Docker also supports these alternative drivers - though you will use the "bridge" driver in most cases:

- host: For standalone containers, isolation between container and host system is removed (i.e. they share localhost as
  a network)
- overlay: Multiple Docker daemons (i.e. Docker running on different machines) are able to connect with each other. Only
  works in "Swarm" mode which is a dated / almost deprecated way of connecting multiple containers.
- macvlan: You can set a custom MAC address to a container - this address can then be used for communication with that
  container.
- none: All networking is disabled.
- Third-party plugins: You can install third-party plugins which then may add all kinds of behaviors and functionalities

As mentioned, the "bridge" driver makes most sense in the vast majority of scenarios.

