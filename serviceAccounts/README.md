# 🔐 Kubernetes ServiceAccounts

> Since Kubernetes **v1.24**, ServiceAccounts **no longer auto-create long-lived tokens** (Secrets).
> Instead, **ephemeral projected tokens** are mounted into Pods automatically if needed.

## 🧠 Key Notes

- **Tokens are no longer stored as Secrets.**
- Pods using a `serviceAccountName` will automatically receive a **short-lived projected token** (via volume mount) if the cluster supports it (enabled by default in most modern clusters like Docker Desktop).
- You must explicitly bind permissions using RBAC (`Role` + `RoleBinding`).

---

## 📌 Step-by-Step Example

### 1. Create a ServiceAccount

```bash
# Create a ServiceAccount
kubectl create serviceaccount myapp-sa -n default
```
### 2. Grant Permissions

```bash
# Create a Role and RoleBinding
kubectl create role pod-reader --verb=get,list --resource=pods -n default
kubectl create rolebinding read-pods-binding \
  --role=pod-reader --serviceaccount=default:myapp-sa -n default
```

### 3. Deploy the Pod

```bash
# Run the pod
kubectl apply -f sa-pod.yaml
```
### 4. Inspect the Token Inside the Pod

```bash
# Check from the terminal
k exec sa-pod -it -- sh
ls /var/run/secrets/kubernetes.io/serviceaccount
cat /var/run/secrets/kubernetes.io/serviceaccount/token
```