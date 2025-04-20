# RBAC

Check user permissions

Imperative commands

```bash
# Create service account
k create sa dev-sa

# Create role and role binding
k create role developers --resource=pod --verb=get,list,watch
k create rolebinding dev-sa-developers --role developers --service-account default:dev-sa
```

```bash
k auth can-i create deployments
k auth can-i delete nodes

# for a particular user
k auth can-i create deployments --as dev-sa
```