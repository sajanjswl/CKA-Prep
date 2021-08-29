## Scheduling

##### Manual Scheduling
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  -  image: nginx
     name: nginx
  nodeName: node01
  ```
##### Lable Selector & Annotations

*   list pods with labels `kubectl get pods --show-labels`
*   list pods with labels bu=finance `kubectl get pods -l bu=finance`
*   list pods with multiple labels `kubectl get pods -l bu=finance,env=prod,tier=frontend`

### Tent and Tolerance
* example mosquito repellent and mosquito
* Tent are set on nodes and Tolerance are set on pods
* Tanting a node `kubectl taint nodes node-name app=blue:NoScheduler` other taint-effect `PreferNoSchedule, NoExecute` 
* When kubernetes cluster is setup a Tantet is placed on the master  node that restict scheduler to schedule any pods `taint-effect=NoSchedule`
* Removing a taint from a node `kubectl taint node master node-role.kuberentes.io/master:NoSchedule`
  
##### PodsWithTolerance

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
  tolerations:
    - key: "app"
      operator: "Equal"
      value: "blue"
      effect: "NoSchedule"  
      
``` 
### Node Affinity Affinity=a natural liking for and understanding of someone/something

* Node Affinity Types: `requiredDuringSchedulingIgnoredDuringExecution, preferredDuringSchedulingIgnoredDuringExecution`

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
  affinity:
   nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
     nodeSelectorTerms:
      - matchExpressions:
         - key: size
           operator: In/Exists/NotIn when Exists is given we don't have to include below values
           values:
            - Small

```

### Resource Request and LimitRange

```
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-limit-range
spec:
  limits:
  - default:
      cpu: 1
    defaultRequest:
      cpu: 0.5
    type: Container
```

## DaemonSet

```
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: monitoring-daemon

spec:
  selector: 
    matchLabels:
      app: montoring-agent
  template:
    metadata:
      labels:
        app: montoring-agent
    spec:
      containers:
        - name: nginx-container
          image: nginx
 ```
### Static Pods
* places pod.yaml files to specfic directory from where it will be read by the kubelet `--pod-manifest-path=/etc/kubernetes/manifests` is the argument provided to the kubelet while running the service
* Another approch while kubeadmin `--config=kubeconfig.yaml` where the contet of kubeconfig.yaml is `staticPodPath:/etc/kubernetes/manifests` 
* kubeadmin tool set up kubernetes contorleplane by installing kubelet  and through static pods
* list static pods: list pods and then look for nodename suffic on podname
* to find the location of pod mainfest run `ps -ef | grep kubelet` and the check for --config  and then cat cofigfile

### Multiple Schedulers

* update the manfigest at `etc/kuberentes/manifest`at master with following parameter `--scheduler-name=my-custom-scheduler` `--lock-object-name=my-custom-scheduler`
* Add `schedulerName:my-custom-scheduler` at container lever in pod.yaml
* How to check which scheduler picks the job `kubectl get events`



### Login && Monitoring
* Node level matric
* pod level matrics
* Monitoring Solutions: metric-server is an inmemory,ELK Stack,Prometheus,DataDog
* cAdvisior is a subcomponent of kubelet: cAdvisor is responsible of retriving performance metrics from pods and expose it through the kubelet api to make the metrics available for metric-server
* Deploying metric-server. Download from github and deploy
* To view metrics of node run `kubectl top node` `kubectl top pod`