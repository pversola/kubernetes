## Step 5: Initialize master node

master single node

```
$ sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --cri-socket=unix:///var/run/crio/crio.sock
```

multi node

```
$ sudo kubeadm init --control-plane-endpoint "<ENDPOINT IP>:<PORT>" --pod-network-cidr=10.244.0.0/16 --upload-certs


$ sudo kubeadm init \
  --pod-network-cidr "10.244.0.0/16" \
  --service-dns-domain "k8s-master" \
  --control-plane-endpoint "k8s-master:6443" \
  --upload-certs
```
