# ClusterMaintainance

* Pod-evication timeout: is set on `kube-controller-manager --pod-evication-timeout=5m0s`. It will evect a pod from the node where the node is not availabe upto 5m0s
* `kubectl drain nodeName` It marks the node as unschedulable and gracefull terminates all the pods on the node.
* `kubectl cordon nodeName` It simply marks the node as unschedulable
* `kubectl uncordon nodeName` It marks the node as schedulable

### Cluster Upgrades.
* With few click on managed-k8s
* With `kubectl upgrade plan` and `kubectl  upgrade apply` when deployed with kubeadm
  
#### Ways to upgrade cluster using kubeadm 
* Check if cluster is setup using kubeadm `kubeadm token list`
* Check the os version and flavour `cat /etc/*release*`
* Fistl of all upgrade the kubeadm itself `apt-get upgrade -y kubeadm=1.2.0-00`
* Then `kubeadm upgrade apply v1.12.0`
* kubeadm runs kubelet on master node to run controlplane components as pods
* After upgrading cluster we still have to upgrade the kubelet manually
* `apt-get upgrade -y kubelet=1.12.0-00`
* `systemctl restart kubelet`
* Next step is to  update the workers node
* The  two best  way possibe to update worker node is to update node one at a time and the other ways is by replacing old node with new node one at a time
* `kubectl drain nodename`
* `apt-get upgrade -y kubeadm=1.12.0-00`
* `apt-get upgrade -y kubelet=1.12.0-00`
* `kubeadm upgrade node config --kubelet-version v1.12.0`
* `systemctl restart kubelet`
  
### etcd-backup
* `ETCDCTL_API=3 etctl snapshot save snapshot.db`
* `ETCDCTL_API=3 etctl snapshot status snapshot.db`
* To restore backupt stop the api-server with `service kube-apiserver stop`
* `ETCDCTL_API=3 etctl snapshot restore snapshot.db --data-dir /var/lib/etcd-from-backup`
* Update the etcd-configuration file to use the new data-dir
* reload the service daemon `systemctl daemon-reload`
* restart etcd service `service etcd restart`
* finally start the kube-apiserver back ` service kube-apiserver start`
```
ETCDCTL_API=3 etcdctl \
 --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key \
  snapshot save /opt/snapshot-pre-boot.db \

// restored snapshot

ETCDCTL_API=3 etcdctl snapshot restore /opt/snapshot-pre-boot.db --data-dir=/var/lib/etcd-from-backup2

then Update the yaml pods for etch present at `/etc/kuberentes/manifests/etcd.yaml`
```
