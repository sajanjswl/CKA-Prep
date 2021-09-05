## Life Cycle Management

#### Rollout & Versioning

* When you first create a deployment it triggers a rollout, a new rollout will create a revision
* To check the rollout status of deployment `kubectl rollout status deployment deploymentName`
* to check histroy or revision run the command `kubectl rollout history deployment deploymentName`
* To undo a change `kubectl rollout undo deployment/mydeployment`
* `kubectl get replicaset`

#### Deployment Startegy
* Recreate : deletes all the pods and then recreate it
* Rolling Update: default deployment strategy. It delete and create pods one by one
  
#### Command & Args command=Entrypoint, args=cmd in Dockerfile
*  first the execuitable and then the parameters CMD["sleep","5"]
* EntryPoint is used to run the execuitable when the container start
* In case of CMD the command line parameter passed entirely replaces the docker CMD Wherease in EntryPoint it is just append
* To set a default parameter use Entrypont to run execuitable and then CMD to pass paramenter
* To overide the entrypoint `kubectl run --entrypoint sleep2.0 nginx 10`
* With args parameter in pod.yaml file we overwrite CMD in docker file
* A `command` field  in pod.yaml represent the entry point in the Dockerfile

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
      command:["sleep2.0"]
      args: ["10"]
```


#### Environment variable in kuberentes configmaps & secrets
* To create a configmap `kubectl create configmap app-config --from-literal=APP_COLOR=blue
* Pay attention configmap in pod/deployet manifests
  
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
      // for normal env
      env:
        - name:
          value:
      // for configmap and secret
      envFrom:
       - configMapRef:
          name: app-config
         
```

```
apiVersion: v1
kind: ConfigMap
metadata: 
    name: app-config
data:
    APP_COLOR: blue
    App_Mode: prod
```

## Secret in kuberentes

* `kubectl create secret generic app-secret --from-literal=DB_HOST=msql`
* To convert secret into base64 encode `echo -n "secret" | base64` to decode `echo -n "secret" | base64 --decode`
```
apiVersion: v1
kind: Secret
metadata: 
    name: app-serect
data:
    DB_HOST: base64enocdedSecret
```

```
Injecting secret as Environment variable
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
      envFrom:
       - secretRef:
          name: app-secret
```

##### Different ways of injecting secret in kuberentes
* ENV:
  ```
   envFrom:
       - secretRef:
          name: app-secret
  ```
* SingleENV:
  ```
   env:
    - name: DB_PASSWORD
      valueFrom: 
       secretKeyRef:
        name: app-secret
        key : DB_PASSWORD
   ```
* Volume:
  ```
    volumes:
   - name: myapp-secret-volume
     secret:
      secretName: app-secret
  ```
  * When secret is used as volume in pods each secret is converted into a file with the value of the secret as it content
  * `ls option/app-secret-volume` will show `DB_HOST DB_Password DB_User`
  
#### Init Containers

```
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container
    image: busybox:1.28
    command: ['sh', '-c', 'echo The app is running! && sleep 3600']
  initContainers:
  - name: init-myservice
    image: busybox
    command: ['sh', '-c', 'git clone <some-repository-that-wil
```