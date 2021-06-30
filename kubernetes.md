## Kubernetes/K8s Architecture
* Helps in Container Orchestration technology

### Containers
* Dockers can be used for creating Containers.
* Check [My Github Notes on Docker](https://github.com/anupama-sinha/anupama-notes/blob/master/docker.md)

### Nodes
* Worker machine where Containers launched by K8s

### Cluster
* Group of nodes
* If one node fails, its accessible through other nodes

### Kube Components
* API Server
* etcd : Key value store
* kubelet 
* Container Runtime
* Controller
* Scheduler 

### POD
* Container encapsulated as PODs and deployed in Node
* A POD can have multi PODs. But its a good practice to have one

### Replication Controllers
* Old approach. Now obsolete
* PODs encapsulated inside Replication Controllers for high availability
* In below POD Definition, apiVersion(v1) & kind(Pod) is skipped, rest all appended in spec-template

```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: myapp-rc
  labels:
    app: myapp
    type: api-service
spec:
  template:
    <<POD Definition>>
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: api-service
    spec:
      containers:
      - name: api-controller
        image: api-docker-image
replicaes: 4
```

### ReplicaSet
* Newer version of Replication Controller used now
* ReplicaSet is defined in apps/v1 version
* Selector-matchLabels is mandatory here as a Replicaset can take care of pre created different PODs also

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-rs
  labels:
    app: myapp
    type: api-service
spec:
  template:
    <<POD Definition>>
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: api-service
    spec:
      containers:
      - name: api-controller
        image: api-docker-image
replicaes: 4
selector:
  matchLabels:
    type: api-service
```

### Upscaling Replicas
* kubectl replace -f replicaset-def.yaml
* kubectl scale --replicas=6 -f replicaset-def.yaml
* kubectl scale --replicas=6 replicaset my-replicaset

### Deployment
* Deployment object created in Node which encapsulates the ReplicaSet
* Below creates a Deployment, ReplicaSet, Service objects

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-rs
  labels:
    app: myapp
    type: api-service
spec:
  template:
    <<POD Definition>>
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: api-service
    spec:
      containers:
      - name: api-controller
        image: api-docker-image
replicaes: 4
selector:
  matchLabels:
    type: api-service
```

### Deployment Rollouts
* Deployment Strategies: Recreate & RollingUpdate(Default)
* Update updates all PODs at once. Not a good approach
* Rollout is better as one POD is updated at a time
* It creates a new ReplicaSet where PODs are deployed either at once or one at a time based on strategy
* Rollout & versioning can be seen with below command
> kubectl rollout status deployment/myapp-deployment
> kubectl rollout history deployment/myapp-deployment
> kubectl rollout undo deployment/myapp-deployment

### Networking
* K8s expects Developer to set the Networks
* All containers/PODs communicate to each other without NAT(Network Address Translation)
* While all nodes can communicate with all Containers without NAT 
* PODs accessible within Nodes
* Service Ports are 30000-32767
* POD has Target Port say 80, While Service has Port(Say 80).And node has NodePort in range 30K

```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: NodePort
  ports:
    - targetPort: 80
      port: 80
      nodePort: 32767
  <<POD Definition to link Service to POD>>
  selector:
    app: myapp
    type: front-end
```

### Multi Services Networking
* Can be done using Cluster IP of AKS


```yaml
apiVersion: v1
kind: Service
metadata:
  name: back-end
spec:
  type: ClusterIP
  ports:
    - targetPort: 80
      port: 80
  <<POD Definition to link Service to POD>>
  selector:
    app: myapp
    type: front-end
```

## References
* Kubernetes for Absolute Beginners(Mumshad Mannambeth), KodeKloud Training
