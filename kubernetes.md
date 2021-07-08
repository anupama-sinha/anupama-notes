## Kubernetes/K8s Architecture
* Helps in Container Orchestration technology

![k8s](https://user-images.githubusercontent.com/68496768/124140154-df8dc000-daa5-11eb-98e6-f3909ce40adc.png)

### Subscription
* ID against which billing is done for Azure resources
* Can have multiple resource groups

### Resource Group
* Logical way of consolidating things
* Helps in setting permission & budget environment can be created based on restrictions(Eg. PROD RG can be 4CPU,16GB, while Non-PROD can be 2GB,8GB)

### Service Connection
* Azure Development Portal(dev.azure.com) & Azure Deployment Portal(portal.azure.com) can be connected using Service Connection(Eg. Azure Resource Manager)

### Steps to connect
* az login
* Then login in Website portal which opens up
* az aks get-credentials --resource-group <<resource-group-name>> --name <<aks-cluster-name>> --subscription <<subscription-name>>
* This creates kube-config in local system and links the same. And then run Docker Desktop before connecting to ACR
* az acr login -n <<acr-name>>
* If Connection Verification to be disabled : SET AZURE_CLI_DISABLE_CONNECTION_VERIFICATION=1

### Alternative Ways to connect
* az account set --subscription "<<subscription-d>>"
* az aks get-credentials --resource-group <<resource-group-name>> --name <<aks-cluster-name>> 
  
### Kubernetes Commands
* Checking servies in specific namespace
> kubectl get services -n <<namespace-name>>
  
* Entering inside POD
> kubectl exec -it <<pod-id>> -n <<namespace-name>> -- bash
  
* Checking Logs
> kubectl logs -f <<pod-id>>

* Command to check descrypted base64 code
> echo <<encrypted-base64>> | base64 -d
  
* Check secret values
> kubectl edit secrets <<secret-name>> -n <<namespace-name>>
  
* Login to ACR
> docker login <<acr-ur>> -u <<acr-username>> -p <<acr-pwd>>
  
* Manually build Docker image & Push in ACR & tag it
> docker build -t demo-application:0.6 .
> docker tag demo-application:0.6 <<acr-name.io>>/demo-application:0.6
> docker push <acr-name.io>>/demo-application:0.6
  
* Get Object Names(pods/configmap/secrets/all/svc/services)
> kubectl get <<object-name>>
  
 * Describe any object
 > kubectl describe pod <<pod-id>>

 * Check last 1hr logs
 > kubectl logs --since=1h pod-id
  
 * Check Top PODs
 > kubectl top pods
 
 * Check Nodes
 > kubectl get nods
  
 * Delete deployment(Service is a logical name of deployment. Not required. But can be done explicitly)
 > kubectl delete deployment <<deployment-name>>
 > kubectl delete service <<service-name>>
  
 * Check ACR Repository List
 > az acr repository list --subscription <<subscription-name>> --name <<acr-name>>
  
 * Check Tags for specific repository
 > az acr repository show-tags --subscription <<subscription-name>> --name <<acr-name>> --repository <<repository-name-from-above-cmd>>
  
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
