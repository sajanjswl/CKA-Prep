#### Security

* To authenticate using basic credentials use `--basic-auth-file=user-details.sv`

##### Generating certificates
* Generates key `openssl genrsa -out ca.key 2048`
* Create a certificate signing request`openssl req -new -key ca.key -subj "/CN=KUBERNETES-CA" -out ca.csr`
* Signs Certificate `openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt`
* for Admin process is same but the csr is signe by ca `openssl x509 -req -in admin.csr  -CA ca.crt -CAkey ca.key  -out admin.crt`
  
#### Viewing certificate 
* if the cluster is setup using kubeadm then look at `cat /etc/kubernetes/manifest/kube-apiserver.yaml`
* to view certtifciate details use `openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout`


#### CSR through API
* encode CSR with `cat akshay.csr | base64
```
apiVersion: certificates.k8s.io/v1beta1
kind: CertificateSigningRequest
metadata:
  name: akshaycsr
spec:
  groups:
  - system:authenticated
  usages:
  - digital signature
  - key encipherment
  - server auth
  requests:
    S0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJQ1ZqQ0NBVDRDQVFBd0VURVBN
QTBHQTFVRUF3d0dZV3R6YUdGNU1JSUJJakFOQmdrcWhraUc5dzBCQVFFRgpBQU9DQVE4QU1JSUJD
Z0tDQVFFQXQ1ZmFMZFJVaExPeGVaYVV2M0FvcXp6dHpNVWFJMHVWd1dWNit4YXFCaUVHCmcwd2sv
ZUh6RDd4T0lFMkxWVUlUVnduTlVpZTR2azNvL2lWZm9UVkIweGJCNDgwK1hOWEFlME1yRktua1FC
a0sKa3BYNm15WHcwelVacWxLMjQ4bVpkbUtHbmV4WFZLYU85eGI3WDVGMlRIbEZGbm1uSjBHMGd6
UDJBQVk0ZExGUApXbEI2akt0MXZLU3dEemxKMzQrYW5oWjB6T29GV3dpN2JySlhoWGl6bkRFd1Bj
T0JuQnBKK0N4U3V1K1lueHFMCklqZW9hYnJwZjh1Y3lLTU5TR1dGT2xsaUtUbnR1MGh3dTVKZEtX
dTZiMFRaT21BQnJPT2RoRkJtVkYxak5PS2YKRy9ScDhxNjZDS3hCdzJ3SGhqNHJORklZMDdyYkhQ
ZXo1RW45aURCbmd3SURBUUFCb0FBd0RRWUpLb1pJaHZjTgpBUUVMQlFBRGdnRUJBSTBsUnJXTG9J
NUVoWklxUkNPNXFPSGM2a01qL1pseVBYUlhGOFlRSXVaMjNRZlJzcTZ1CmVoV0hzUFFBZCtraTJZ
Y1Z4aWNCM2JIN0E1RC9ESlY1aXdGWWRscFRhdEZmdU16UksyZHNPQ2JZZ21Cb3h5RUEKUUhpYXpL
cFg0V0Nsd1JpeDVDaG82L291UjFPWmxBS3I2bmZobWpadUppZG8xV00vVGM5UFlWSXlRaW1NZVBR
OAo4ZlJRWDNyeUJ3MVMrL1NNVXNCZWd5cU1JdFJ0UmtieEJ2d01WZEwzVTZPNExHVGovNHl1ei9r
ZjVIb3lUYlpUCjdXY255YUJGTSttV0w3ZDdmRlhVK1JGUlVwMVdNUnk4Y1BKd2gyeW54amlaYkhI
TEZ2dVlYaUEyajlWY2lrQ0QKRCtNZ3JmSEE5dS9IQ0lCZVA3ZFFOM0MyWGdmNjlrekw4dkk9Ci0t
LS0tRU5EIENFUlRJRklDQVRFIFJFUVVFU1QtLS0tLQo=
```

#### kubeconfig




### ApiGroups 
* to view the groups `kubectl proxy` & `curl http://localhost:8001 -k` 


### RBAC
```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-reader
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
```
* to check access `kubectl auth can-i create deployments --as dev-user`
