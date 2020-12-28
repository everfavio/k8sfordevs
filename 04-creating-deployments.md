# Creating Deployments
- deployment core concepts
- creating a deployment
- kubectl and deployments
- deployment options

## deployments core concepts

A ReplicaSet is a declarative way to manage pods
A Deployment is a declarative way to manage pods using a ReplicaSets

remembering:

Pods represent the most basic resource in kubernetes
can be created and destroyect but are never recreated

What happends if a Pod is destroyed?

Deployments and ReplicaSets enure Pods stay running and can be used to scale Pods

### the role of ReplicaSet

Replicasets act as a Pod controller
- self healing mechanism
- Ensure the requested number of pods are available
- Provide fault-tolerance
- Can be used to scale pods
- Relies on a pod template
- Used by deployments

### the role of deployment
Deployment manages pods:
- pods are managed using replicaSets
- Scales replicaSets, wich scale pods
- supports zero-downtime updates by creating and destroying replicaSets
- provides rollback funcionality
- Creates a unique label that is assigned to the replicaSet and generated Pods
- YAML is very similar to a replicaSet

## Creating a deployment
```yaml
# example 1
apiVersion: v1                    # kubernetes api version and resource type (deployment)
kind: Deployment
metadata:                         # Metadata about the deployment
spec:
  selector:                       # select pod template label(s)
  template:                       # template used to create the pods
    spec:
      containers:
      - name: my-nginx
        image: nginx:alpine
```

```yaml
# example2
apiVersion: apps/v1               # kubernetes api version and resource type (deployment)
kind: deployment
metadata:                         # metadata about the deployment
  name: frontend
  labels:
    app: my-nginx
    tier: frontend
spec:
  selector:                       #the selector is used to select the template to use(based on labels)
    matchLabels:
      tier: frontend
  template:
    metadata:
      labels:                     #template to use t ocreate the Pod/containers (note that the selector matches the label)
        tier: frontend
    spec:
      containers:
      - name: my-nginx
        image: nginx:alpine
```
## kubectl and deployments

kubectl create -f deployment.yaml  -> create a deployment
kubectl  apply -f deployment.yaml  -> alternative way to create or apply changes to a deployment.yaml file

--save-config  -> Use when you want to use kubectl apply in the future
kubectl get deployments -> getting deployments
--show-labels -> List all deployments and their labels
-l <label> -> to get information about a deployment with specific label
kubectl delete deployment <name> -> delete a deployment
kubectl scale deployment [deployment-name] --replicas=5  -> scale replicaSet of deployment
kubectl scale -f deployment.yaml --replicas=5 -> same but with a file

## deployment options


