# Kubernetes in Action

## Your Work / Kubernetes' Work

What Kubernetes Will Do

- Create your objects (e.g Pods) and manage them
- Monitor Pods and re-create them, scale Pods etc.
- Kubernetes utilizes the provided(cloud) resources to appy your configuration/goals

What you need to do/setup

- Create the Cluster and the Node Instances (Worker + Master nodes)
- Setup API Server, kubelet and other kubernetes services/software on Nodes
- Create other (cloud) provider resources that might be needed.

## Understanding Kubernetes Objects

Kubernetes works with Objects ;

- Pods
- Deployments
- Services
- Volume

Objects can be created in two ways : Imperatively or Declaratively

## The "Pod" Object

The smallest unit Kubernetes interacts with ;

- Contains and runs one or multiple containers.
    - The Most common use-case is "one container per Pod".
- Pods contain shared resources (e.g volumes) for all Pod containers.
- Has a cluster-internal IP by default.
    - Containers inside a Pod can communicate via localhost.

Pods are designed to be ephemeral. Kubernetes will start, stop, and replace them as needed.
For Pods to be managed for you , you need a "Controller" (e.g a "Deployment")

## The "Deployment" Object

Controls (multiple) Pods

- You set a desired state, Kubernetes then changes the actual state.
    - Define which Pods and containers to run and the number of instances.
- Deployments can be paused, deleted and rolled back.
- Deployments can be scaled dynamically (and automatically).
    - You can change the number of desired Pods as needed.

Deployments manage a Pod for you, you can also create multiple deployments.

You therefore typically don't directly control Pods, instead you use Deployments to set up the desired end state.

    kubectl create deployment first-app --image=leventerguder/kub-first-app

    kubectl get deployment
    kubectl get pods

    minikube dashboard

    kubectl delete pods ..
    kubectl delete deployment first-app

### Behind The Scenes

    kubectl create deployment --image ...

Master Node (Control Plane)

- Scheduler analyzes current running Pods and finds the best Node for the new Pod(s).
- kubelet manages the Pod and Containers.

## The "Service" Object

Expose Pods to the Cluster or Externally.

- Pods have an internal IP by default. It changes when a Pod is replaced.
    - Finding Pods is hard if the IP changes all the time.
- Services group Pods with a shared IP.
- Services can allow external access to Pods.
    - The default (internal only) can be overwritten.

Without Services, Pods are very hard to reach and communication is difficult.

Reaching a Pod from outside the Cluster is not possible at all without Services.

    kubectl expose deployment first-app --type=LoadBalancer --port=8080
    kubectl get services
    minikube service first-app

A replica is simply an instance of a Pod/Container. 3 Replicas means that the same POD/container is running 3 times.

    kubectl scale deployment/first-app --replicas=3
    kubectl get pods
    kubectl scale deployment/first-app --replicas=1



    docker build -t leventerguder/kub-first-app:2 .
    docker push leventerguder/kub-first-app:2

    kubectl set image deployment/first-app kub-first-app=leventerguder/kub-first-app:2
    kubectl rollout status deployment/first-app

    kubectl delete service first-app
    kubectl delete deployment first-app

## Resource Definition

## Imperative vs Declarative Kubernetes usage

Imperative

- kubectl create deployment ...
- Individual commands are executed to trigger certain Kubernetes actions
- Comparable to using docker run only

Declarative

- kubectl apply -f config.yaml
- A config file is defined and applied to change the desired state.
- Comparable to using Docker Compose with Compose files.

    
    kubectl apply -f=deployment.yaml
    kubectl delete   -f=deployment.yaml -f=service.yaml