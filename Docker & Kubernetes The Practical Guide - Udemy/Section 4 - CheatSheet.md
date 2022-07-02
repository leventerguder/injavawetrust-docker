# Networks / Requests

In many application, you'll need more than one container - for two main reasons

- It's considered a good practice to focus each container on one main task (e.g. run a web
  server, run a database, ...)
- It's very hard to configure a Container that does more than one "main thing" (e.g. run a
  web server AND a database)

Multi-Container apps are quite common, especially if you're working on "real applications".

Often, some of these Containers need to communicate though:

- either with each other
- or with the host machine
- or with the world wide web

## Communicating with the WWW

This will work out of the box, no extra configuration is required. The application, running in a Container,will have no
problems sending this request.

## Communicating with the Host Machine

Communicating with the Host Machine is also quite simple, though it doesn't work without any changes.

One important note: If you deploy a Container onto a server (i.e. another machine), it's very unlikely
that you'll need to communicate with that machine. Communicating to the Host Machine typically is a
requirement during development - for example because you're running some development database on
your machine.

    fetch('localhost:3000/demo').then(...)

This code snippet tries to send a GET request to some web server running on the local host
machine (i.e. outside of the Container but not the WWW).

On your local machine, this would work - inside of a Container, it will fail. Because localhost inside of the Container
refers to the Container environment, not to your local host machine which is running he Container / Docker

host.docker.internal is a special address / identifier which is translated to the IP address of the machine hosting the
Container by Docker.

## Communicating with Other Containers

Communicating with other Container is also quite straightforward. You have two main options :

- Manually find out the IP of the other Container.
- Use Docker Networks and pout the communicating Container into the same Network.

Option 1 is not great since you need to search for the IP on your own and it might change over
time.

Option 2 is perfect though. With Docker, you can create Networks via docker network create
SOME_NAME and you can then attach multiple Containers to one and the same Network.

    docker run -network my-network --name cont1 my-image
    docker run -network my-network --name cont2 my-other-image

Both cont1 and cont2 will be in the same Network.
Now, you can simply use the Container names to let them communicate with each other , again Docker will resolve the Ip
for you.