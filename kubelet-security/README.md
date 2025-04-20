# Kubelet Security
The Kubelet is the agent running on each node that manages Pods. If misconfigured, it can allow attackers to execute commands or get sensitive data.

## Main Steps for Securing the Kubelet

- 🔐 Enable authentication (--authentication-token-webhook=true)
- ✅ Enable authorization (--authorization-mode=Webhook)
- 🚫 Disable anonymous access (--anonymous-auth=false)
- 🔒 Use TLS for Kubelet server (--tls-cert-file, --tls-private-key-file)
- 🛑 Restrict read-only and insecure ports (--read-only-port=0, --insecure-port=0)
- 📜 Set --protect-kernel-defaults=true
- 🔍 Use CIS Benchmark to validate settings

## Example

You want to harden the kubelet so users can’t just curl :10250/pods and get sensitive data or exec into pods without proper authentication.

Step 1 : Copy the file `config.yaml` to `/var/lib/kubelet/config.yaml`

```bash
cp config.yaml /var/lib/kubelet/
```

Step 2: Restart kubelet

```bash
systemctl daemon-reexec
systemctl restart kubelet
```

Test

```bash
curl -k https://localhost:10250/pods  # Should return 403 Forbidden if auth is working
curl -k http://localhost:10255/metrics  # Should fail if read-only port is disabled correctly
```

##💡 Optional Tip or Gotcha

On CKS exam environments, run ps aux | grep kubelet to inspect actual kubelet flags if /var/lib/kubelet/config.yaml is not used.

## References

- [Kubernetes Kubelet Configuration Docs](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/)
- [CIS Kubernetes Benchmark](https://www.cisecurity.org/benchmark/kubernetes)