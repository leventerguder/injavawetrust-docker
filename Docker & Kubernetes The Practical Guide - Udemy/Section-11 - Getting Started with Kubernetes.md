**# Getting Started with Kubernetes

Manuel deployment of Containers is hard to maintain, error-prone and annoying(even beyond security and configuration
concerns)

- Containers might crash/go down and need to be replaced.
- We might need more container instances upon traffic spikes.
- Incoming traffic should be distributed equally.

Services like AWS ECS ;

- Container health checks + automatic re-deployment
- Autoscaling
- Load Balancer

But that locks us in !

- Using a specific cloud service locks us into that service.
- You need to learn about the specifics, services and config options of another provider if you want to switch.
- Just knowing Docker isn't enough!

Kubernetes ;

An open-source system (and de-facto standard) for orchestrating container deployments.

- Automatic deployment
- Scaling & Load Balancing
- Management

Kubernetes Configuration ---> Some Provider - Specific Setup or Tool -> Any Cloud Provider or Remote Machines.

## What Kubernetes IS and IS NOT

- It's not a cloud service provider.
- It is an open-source project.
- It is not a service by a cloud service provider.
- It can be used with any provider.
- It is not restricted to any specific cloud service provider.
- It can be used with any provider.
- It is not just a software you run on some machine.
- It is a collection of concepts and tools.
- It is not an alternative to Docker
- It works with (Docker) containers.
- It is not a paid service.
- It is a free open source project.

Kubernetes is like Docker-Compose for multiple machines.

## Core Kubernetes Concepts & Architecture

You can think of a pod as the smallest possible unit in the Kubernetes world.

- Worker Nodes run the containers of your application.
- Nodes are your machines/virtual instances.
- Multiple Pods can be created and removed to scale your app.
- The Master Node controls your deployment (Worker Nodes)

## Your Work/ Kubernetes' Work

What you need to do/setup

- Create the Cluster and the Node Instances ( Worker + Master Nodes)
- Setup API Server , kubelet and other Kubernetes services/software on Nodes.
- Create other (cloud) provider resources that might be needed. (e.g Load Balancer, Filesystems)

What Kubernetes Will Do

- Create your objects (e.g Pods) and manage them
- Monitor Pods and re-create them, scale Pods etc.
- Kubernetes utilizes the provided (cloud) resources to apply your configuration/goals.

Worker Node :Think of it as one computer/machine/virtual instance.
The Worker Node is managed by the Master Node

Pods are managed by Kubernetes.
You could also have multiple Containers inside of a Pod.

Hosts one or more application containers and their resources (volumes , IP , run config)
Pods are created and managed by Kubernetes.

kubelet = Communication between Master and Worker Node
kube-proxy = Managed Node and Pod network communication

Master Node;

- API Server ; API for the kubelets to communicate.
- Scheduler ; Watches for new Pods, select Worker Nodes to run them on.
- Kube-Controller-Manager ; Watches and controls Worker Nodes, corrects number of Pods & more
- Cloud-Controller-Manager ; Like Kube-Controller-Manager BUT for a specific Cloud Provider:
  Knows how to interact with Cloud Provider resources

## Core Components

- Cluster ; A set of Node machines which are running the Containerized Application(Worker Nodes) or control other
  Nodes.(Master Node)
- Nodes ; Physical or virtual machine with a certain hardware capacity which hosts one or multiple Pods and communicates
  with the Cluster.
- Master Node ; Cluster Control Plane , managing the Pods across Worker Nodes
- Worker Node ; Hosts Pods , running App Containers (+resources)
- Pods ; Pods hold the actual running App Container + their required resources (e.g volumes)
- Containers; Normal(Docker) containers.
- Services ; A logical set (group) of Pods with a unique, Pod-and Container- independent IP address.