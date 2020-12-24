# Introduction
Some scenarios to use kubernetes

Kubernetes Overview
The big picture
benefits and use cases
Runing kubernetes locally
getting started with kubectl
Web UI Dashboard

## Kubernetes Overview
Kubernetes is an open.source system for automating deployment, scaling, and management of containerized applications

kubernetes.io

for example
Load balancer example

It wold be nice if we could...
Package up an app and let something else manage it for us
Not worry about the management of containers
Eliminate single points of failure
scale containers easly
Update containers without bringing down the application
Have robust networking and persistent storage options

all of this mencioned thing can be possible with kubernetes

kubernetes is the conductor of a container orchesta

### key kubernetes Feature

- Service discovery / Load Balancing
- Storage Orchestration
- Automate Rollouts / Rollbacks
- Self-healing
- Secret and configuration management
- Horizontal scaling
- ... and even more

## the big picture

- Container and cluster management
- Open Source Project
- Used internally bi google for 15+ years and donated to the cloud native computing foundation
- Supportend by al major cloud platforms
- provides a "declarative" way to define a cluster's state

kubernetes moves you to a desired state
(current state -> desired state)

This have a master node who is controling all of worker nodes
The pod is the packaging of the container
pod
service
deployment
replicaSet

etcd (store or database)
Controller manager
Scheduler
Api Server

Kubectl

Kubelet -> agent running in every node
Kube-proxy -> network capabilites

## benefits and use cases
### Key container benefits
- accelerate developer onboarding
- Eliminate app conflicts
- environment consistency
- ship software faster

### Key kubernetes benefits
- orchestrate containers
- Zero-downtime deployments
- Self healings superpowers
- scale containers

### Developer use cases
- Emulate production locally
- Move from docker compose to kubernetes
- Create an end-to-end testing environment (for example selenium)
- Ensure application scale properly
- ensure secrets/config are working properly
- workload scenarios (CI/CD and more)
- learn how to leverage deployment options
- help DevOps create resources and solve problems

## Runing kubernetes locally
- best option -> Minikube you can use that with docker or vmware on background
  limitations ->
    - you cannot scale like a production
    - you only have a single node
- another option is Kind
- if you want to install fully kubernetes you can use kubeadm

## Getting started with kubectl
Kubectl command line tool
interact with api on the master node

kubectl version         -> get kubernetes version
kubectl cluster-info    -> view cluster information
kubectl get all         -> Retrieve information about kubernetes pods, deployments, services and more...
kubectl run [container-name] --image=[image-name]  -> simple way to create a deployment for a pod
kubectl port-forward [pod] [ports]  -> Forward a port to allow external access
kubectl expose ...      -> EXpose a port for a deployment/pod
kubectl create [resource] -> create a resource
kubectl apply [resource] -> Create or modify a resource

## Web UI dashboard
(use the k8s official documentation)
Web ui dashboard provides a user interface to view kubernetes resources
Steps to enable the ui Dashboard:
  - kubectl apply [dashboard-yaml-url]
  - kubectl describe secret -n kube-system
  - locate the kubernetes.io/service-account-token and copy the token
  - kubectl proxy
  - visit the dashboard URL and login using the token


