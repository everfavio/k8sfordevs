# Creating services
- Services core concepts
- services types
- creating a service with kubectl
- creating a service with YAML


## Service core concepts

A service provides a single point of entry for accesing one or more pods.

Review Question:
Since pods live and die, can you rely on their IP
Answer:
No! that's why we need services- IPs change a lot!

#### the life of a Pod

- Pods are mortal, and may only live a short time (ephemeral)
- You can't rely on a Pod IP address staying the same
- Pods can be horizontally scaled so each pod gets its own IP address
- A pod gets an IP address after it gas ben scheduled(no way for clientes to know ip ahead of time)

#### The role of services

- Services abstrat Pod IP addresses form consumers
- load balances between pods
- Relies on labels to associate a service with a pod
- Node's kube-proxy creates a vurtual IP for services
- Layer 4 (tcp/udp) over ip
- services are not ephemeral
- Creates ebdoiubts which sit between a service and pod

## Services type
Services cam be defined in differents ways:

- ClusterIP - Expose the service on a cluster-internal IP (default)
- NodePort - expose the service on each node's IP at a static port.
- LoadBalancer - provision an external ip to act as load balancer for the service
- ExternalName . maps a service to a dns name

#### cluster ip service
- Service Ip is exposed internally within the cluster
- only pods within the cluster can talk to the service
- allows pods to talk to other pods

#### NodePort service
- Exposes the service on each node's IP at a static port
- allocates a port from a range (default is 30000-32767)
- each node proxies the allocated port

#### Load Balancer service
- Exposes a service externally
- Useful when combined with a cloud providers load balancer
- nodeport and clusterIP services are created
- each node proxies the allocated port

#### ExternalName Service
- Service that acts as an alias for an external service
- allows a service to act as the proxy for an external service
- External service details are hidden from cluster (easier to change)

## Creating a service with kubectl
#### Port forwarding
Q How can you access a pod from outside of kubernetes?
R. por forwarding

Use the kubectl port-forward to forward a local port to a pod port
```shell
# listen on port 8080 locally and forward to port 80 in pod
$ kubectl port-forward pod/[pod-name] 8080:80
```
```shell
# listen on port 8080 locally and forward to deployment's pod
$ kubectl port-forward deployment/[deployment-name] 8080:80
```
```shell
# listen on port 8080 locally and forward to service's pod
$ kubectl port-forward service/[service-name] 8080:80
```
## Creating a service with YAML
```yaml
apiVersion: v1     # kubernetes api version and resource type (service)
kind: Service
metadata :         # metadata about the service
spec:
  type:            # Type of service (clusterIP, NodePort, LoadBalancer) - defaults to ClusterIP
  selector:         # select pod template label(s) that service will apply to
  ports:            #Define container target port and the port for the service
```
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  selector:         # service will apply to resources with a label of app: nginx
    app: nginx
  ports:
  - name: http
    port: 80
    targetPort: 80
```
#### conectiong to a service by it's dns name
```yaml
apiVersion: v1
kind: Service
metadata:              #Name of service (each service gets a dns entry)
  name: frontend
---
apiVersion: v1
kind: Service
metadata:              # a frontend pod can access a backend pod using backend:port
  name: backend
```

#### creating a nodeport service
```yaml
apiVersion: v1
kind: Service
metadata:
  ---
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
  - port: 80
    targetPort: 80
    nodePort: 31000
```
#### creating a loadbalancer service
```yaml
apiVersion: v1
kind: Service
metadata:
  ...
spec:
  type: LoadBalancer    # Ser service type to loadBalancer (normally used with cloud providers)
  selector:
    app: nginx
  ports:
  - port: 80
    targetPort: 80
```
#### creating an externalname service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: external-service          # other pods can use this FQDN to access the external service
spec:
  type: ExternalName              # set type to ExternalName
  externalName: api.acmecorp.com  # Service will proxy to FQDN
  ports:
  - port: 9000
```
## kubectl and services
#### Creating a service
Use the kubectl create command along with the --fileame or -f switch
```shell
## create a service
$ kubectl create -f file.service.yaml
## update a service
## assumes --save-config was used with create
$ kubectl apply -f file.service.yml
## delete a service
$ kubectl delete -f file.service.yml
```
#### testing a service and pod with curl
how can you quickly test if a service and pod is working
use kubectl exec to shell into a pod/container
```shell
# shell into a pod and test a URL. Add -c [containerID]
# in cases where multiple containers are running in the Pod
$ kubectl exec [pod-name] -- curl -s http://podIP
# install and use curl (example shown is for alpine linux)
$ kubectl exec [pod-name] -it sh
> apk add curl
> curl -s http://podIP
 ```

## summary

- pods live and die so their ip address canb change
- Services abstract pod ip addresss from consumers
- Labels associate a service with a pod
- Service types:
  - ClusterIp (internal to cluster-default)
  - NodePort (expose service on each's node's IP)
  - Load Balancer (exposes a service externally)
  - ExternalName (proxies to an external service)

Services in general and networking are really big topics.

