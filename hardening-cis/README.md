# Cluster Setup and Hardening

## CIS Benchmarks
CIS Kubernetes Benchmark is a set of security configuration best practices to harden Kubernetes components like etcd, kubelet, kube-apiserver, etc. You must assess and fix insecure settings to meet the benchmark.

### Tools
#### `kube-bench` tool (by Aqua Security).
kube-bench audit against the CIS Benchmark. It scans your cluster for known insecure default configurations and suggests remediations.

Run as Job
```bash
 kubectl apply -f https://raw.githubusercontent.com/aquasecurity/kube-bench/main/job.yaml

 # Check logs
 kubectl logs -l job-name=kube-bench

 # More details from logs
k logs kube-bench-qdcgk | less
k logs kube-bench-qdcgk | grep FAIL

```

 Run from binary file https://github.com/aquasecurity/kube-bench/releases
```bash
# Install
wget https://github.com/aquasecurity/kube-bench/releases/download/v0.10.4/kube-bench_0.10.4_darwin_amd64.tar.gz
tar -xvf kube-bench_0.10.4_darwin_amd64.tar.gz

# run a test
./kube-bench --config-dir `pwd`/cfg --config `pwd`/cfg/config.yaml
./kube-bench -D cfg
```

 #### CIS-CAT Pro Assessor tool called Assessor-CLI
 The CIS-CAT Pro Assessor CLI is a command-line tool from the Center for Internet Security (CIS) that automates the evaluation of system configurations against CIS Benchmarks, including Kubernetes. It’s used to validate compliance and hardening based on best practices.

``` bash
 sh ./Assessor-CLI.sh -i -rd /var/www/html/ -nts -rp index
 ```