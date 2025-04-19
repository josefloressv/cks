# TLS - Kubernetes Certificates

Kubernetes uses TLS certificates for secure communication between its components (e.g., kubelet, API server, etcd) and also for user and service authentication via client certificates.

---

## 🔹 Main Steps When Working with TLS in Kubernetes

1. Generate a private key and CSR (or self-signed certificate)
2. Approve or sign the CSR (using Kubernetes CA or cert-manager)
3. Retrieve and distribute the signed certificate
4. Configure Kubernetes components to use the cert
5. Optionally create a `kubeconfig` for user access

---

## 🛠️ End-to-End Example: Client Cert Authentication

We'll create a private key and CSR, submit it to Kubernetes, approve it, and use the signed certificate to authenticate.

### Step 1: Generate Private Key and CSR

```bash
openssl genrsa -out dev.key 2048
openssl req -new -key dev.key -out dev.csr -subj "/CN=dev/O=devs"
```

📌 `CN` is the username, `O` is the group—these must match your RBAC bindings.

#### Optional: Inspect the CSR

```bash
openssl req -in dev.csr -text -noout | less
```

---

### Step 2: Create Kubernetes CSR Resource

Encode the CSR and remove newlines to prepare it for YAML:

```bash
cat dev.csr | base64 | tr -d '\n'
```

Insert the base64-encoded value in a `CertificateSigningRequest` resource, then apply it:

```bash
kubectl apply -f dev-csr.yaml
```

---

### Step 3: Approve the CSR

```bash
kubectl get csr
kubectl certificate approve dev-csr
```

---

### Step 4: Retrieve the Signed Certificate

```bash
kubectl get csr dev-csr -o jsonpath='{.status.certificate}' | base64 -d > dev.crt
```

#### Optional: Validate the Certificate

```bash
openssl x509 -in dev.crt -text -noout | less
```

---
### Step 5: Create Role and Role Binding

```bash
kubectl apply -f dev-ns.yaml
kubectl apply -f dev-role.yaml
kubectl apply -f dev-rolebinding.yaml
```
---
### Step 6: Create `kubeconfig` Entry for the User

```bash
k config get-cluster
kubectl config set-credentials developer --client-certificate=dev.crt --client-key=dev.key
kubectl config set-context developer-context --cluster=docker-desktop --namespace=dev --user=developer
kubectl config use-context developer-context
```
`--user=developer` *is local to kubeconfig, like an alias*

Test the access

```bash
# With user context set:
kubectl get pods -n dev           ✅ should work
kubectl get pods -n kube-system   ❌ forbidden
```
---

## 💡 Tip

You must bind the user (`CN=dev`) to a Role or ClusterRole for any permissions to work:

```bash
kubectl create rolebinding dev-access --clusterrole=view --user=dev -n default
```

Otherwise, you'll get an `unauthorized` error.

---
