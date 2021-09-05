# CKA

##Core Concepts
* Default etcd listen address `2379`

### Pod
```
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels: 
    app: myapp-pod
    type: frontEnd
spec:
  containers:
    - name: my-app
      image: nginx         
```

* kinds & versions
```
Pod             v1
Service         v1
ReplicaSet      apps/v1
Deployment      apps/v1
DaemonSet       apps/v1
StatefulSet     apps/v1
```
* writing to a file cat > pod.yaml  Enter and then ctr+P
* create a pod : `kubectl run nginx --image nginx`
* generating yaml from dry-run `kubectl run redis --image=redis --dry-run=client -o yaml >tem.yaml`


### ReplicaSet
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-replicaset
  labels:
    type: front-end
spec:
  template:
    metadata:
      name: myapp-pod
      labels:
         type: front-end
    spec:
      containers:
        - name: nginx-container
          image: nginx
  replicas: 3
  selector: 
    matchLabels:
      type: front-end
```
* `kubectl scale --replicas 2 replicaset myapp-replica`
  
### Deployment
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-replicaset
  labels:
    type: front-end
spec:
  template:
    metadata:
      name: myapp-pod
      labels:
         type: front-end
    spec:
      containers:
        - name: nginx-container
          image: nginx
  replicas: 3
  selector: 
    matchLabels:
      type: front-end
```
* `kubectl create deployment somename --image nginx`
* `kubectl scale deployment somename --replicas 3`
* creating deployment yaml file `kubectl create deployment --image httpd:2.4-alpine httpd-frontend --replicas 3 --dry-run=client -o yaml > t1.yaml`
  
### Namespace

* To connect to a service in the same namespace `mysql.connet(db-service)
* To connect to a pod in default namespace to a database in dev namespae use `mysql.connect("db-service.dev.sv.cluster.local")' 
*  db-service= service name
*  dev=namespace
*  svc=service
*  cluster.local=domain
*  switching betwwen namespaces `kubectl config set-context $(kubectl current context) --namespace=dev`
#### ResouseQuotas
```
apiVersion: v1
kind: ResourceQuota
metadata:
  name: resourcequota-dev
  namespace: dev
spec:
  hard:
    pods: "2"
    requests.cpu: "4"
    requests.memory: 5Gi
    limits.cpu: "10"
    limits.memory: 10Gi
```

### Services
* Types: NodePort ClusterIP, LoadBalancer
##### NodePort
```
apiVersion: v1
kind: Service
metadata:
  name: node-port-service
spec:
  type: NodePort
  ports:
  // targetPort means pod port
    - targetPort: 80
  //  port means service port
      port: 80
      nodePort: 30008
  selector:
      type: front-end
  ```

  #### ClusterIP or default service

```
apiVersion: v1
kind: Service
metadata:
  name: cluster-ip-service
spec:
  type: ClusterIP
  ports:
  // targetPort means pod port
    - targetPort: 80
  //  port means service port
      port: 80
  selector:
      type: front-end
```
###### Note Imperative vs Declarative
```
Imperative                                Declarative
kubectl create -f nginx.yaml              kubectl apply -f nginx.yaml
kubectl replace -f nginx.yaml             kubectl apply -f nginx.yaml
```

* `kubectl expose pod redis --name redis-service --port 6379 --target-port 6379 `
* creates both service and pods`kubectl run nginx --image nginx --port 80 --expose`
  