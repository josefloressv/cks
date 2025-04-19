# cks
CKS notes for study the Certification content

## Concepts
### 4C's of Cloud Native

* Cloud
* Cluster
* Container
* Code

### Authentication vs Authorization
* Authentication: Confirms who you are.
* Authorization: Determines what you are allowed to do.

Authentication: Setup and use mechanisms like:
- Certificates
- Bearer Tokens
- OIDC
- Service Accounts

Authorization: Control access using:
- RBAC (Role-Based Access Control)
- ABAC (Attribute-Based Access Control)
- Webhook Authorizers

### TLS
TLS (Transport Layer Security) encrypts communication between Kubernetes components (like kubelet, API server, etc.). Certificates are used to authenticate and secure this communication.

Kube API Server
/etc/kubernetes/manifests/kube-apiserver.yaml

Info of a certificate

```sh
openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout
```

```sh
journalctl -u etcd.service -l
kubectl logs etcd-master
```