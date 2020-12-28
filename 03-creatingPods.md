# Creating pods

Pod Core Concepts
Creating a pod
Kubectl and Pods
Yaml fundamentals
Defining a pod with YAML
Pod Health

## Pod Core Concepts

A pod is the basic execution unit of a kubernetes application. The smallest and simplest unit in the kubernetes object model that you create or deploy.

Smallest object of fthe kubernetes object model

Environment for containers

Organize applications parts into pods (server, caching, apis, database, etc).

Pod Ip,memory, volumes, etc. shared across containers.

Scale horizontally by adding Pod replicas

Pods live and die but never come back to life

## Creating a pod
there are several different ways to schedule a Pod:

kubectl run command
kubectl create/apply command with a yamlfile

example:
kubectl run [podname] --image=nginx:alpine

### get information about the pods

kubectl get pods -- list only pods

kubectl get all -- list all resources

The kubectl get command can be used to retrieve information about pods and many others kubernetes objects.
### expose a pod port
Pods and containers are only accesible within the kubernetes cluster by default

One way to expose a container port externally: Kubectl port-forward

### Enable pod container to be called externally
Kubectl port-forward [name-of-pod] 8080:80

### Will cause pod to be recreated
kubectl delete pod [name-of-pod]
Runbning a pod cause a deployment to be created
to delete a pod use kubectl delete pod or find the deployment and use kubectl delete deployment

### delete deployment that manages the pod
kubectl delete deployment [name-of-deployment]

## kubectl and pods

kubectl run my-nginx --image=nginx:alpine
kubectl port-forward [my.pod] 8080:80

## YAML fundamentals
yaml files are composed of maps and lists
always use spaces
maps:
  - name: value pairs
  - maps can contain other mas for more complext data struncture
List:
  - sequence of items
  - Multiple maps can be defined in a list

example
```yaml
key: value
complextMap:
key1: value
key2:
  subkey: value
items:
  - item1
  - item2
itemsMap:
  - map1: value
    map1prop: value
  - map2: value
    map2prop: value
```
## Defining a Pod with yaml

```yaml
apiVersion: v1              # kubernetes api version
kind: Pod                   # type of kubernetes resource
metadata:                   # metdata about the pod
  name: my-nginx
spec:                       # the spec/plueprint for the pod
  containers:               # Information about the containers that will run in the Pod
  - name: my-nginx
    image: nginx:alpine
```
To create a pod using a yaml use the kubectl create command along with the --filename or -f switch

kubectl create -f file.yaml --dry-run --validate=true
--dry-run is only for test the implementation, if everything is ok k8s will return an succesfull message without implementing o creating any resource

--validate flag reviews the yaml file before the implementation, true is default value
--save-config store current properties in resource's annotations

having this allow in-place changes to be made to a pod in the future using kubectl apply
```yaml
apiVersion: v1
kind: Pod
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration:
      {
        "apiVersion":v1, "kind":"pod",
        "metadata":{
          "name": "my-nginx"
          ...
        }
      }
```
### delete a pod
kubectl delete pod [name-of-pod]
kubectl delete -f file.pod.yaml

## kubectl and yaml

kubectk exec my-nginx  -it sh
kubectl describe pod [pod-name]
kubectl apply --
kubectl exec --
kubectl delete -f --
## Pod Health

Kubernetes relies on probes to determine the health of a pod container.

A probe is a diagnostic performed periodically by the kubelet on a container

### types of probes
  - liveness probe
    can be used to determine if a Pod is healthy and running as expected
  - readinesprobe
    can be used to determine if a Pod should receive request
  - Failed pod containers are recreated by default (restartPolicy defaults to always)

ExecAction - executes an action inside the container

TCPSocketAction - TCP check against the container's IP address on a specified port

HTTPGetAction - HTTP GET request against container

Probes can have the following result:
- success
- Failure
- Unknow

```yaml
apiVersion: v1
kind: Pod
...
spec:
  containers:
  - name: my-nginx
    image: nginx:alpine
    livenessProbe:            #define liveness probe
      httpGET:
        path: /index.html     # check /index.html on port 80
        port: 80
      initialDelaySeconds: 15 # wait 15 seconds
      timeoutSeconds: 2       # timeout after 2 seconds
      periodSeconds: 5        # check ever 5 seconds
      failureThreshold: 1     # allow 1 failure before failing pod
```

Readiness probe:
when should a container start receiving traffic?

liveness probe:
when should a container restart?

# summary
pod are the smallest unit of kubernetes

Containers run within pods and share a pod's memory. IP. volumes, and more

pods can be started using different kubectl commands

yaml can be usedto create a pod

health checks provide a way to notify kubernetes when a pod has a problem.


