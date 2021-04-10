## Step 8: Add worker nodes

```
$ sudo kubeadm join <ENDPOINT IP>:<PORT> --token <TOKEN> --discovery-token-ca-cert-hash sha256:<DISCOVERY TOKEN>
```

Configure label

```
kubectl label node <WORKER NODE> kubernetes.io/role=worker1
kubectl label node <WORKER NODE> kubernetes.io/role=worker2
```

#### How to manual join node **(Token Expire)**

Step 1: Get Token

```
$ sudo kubeadm token list
$ sudo kubeadm token create --print-join-command
```

Step 2: Get Discovery Token CA cert Hash

```
$ openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin -outform der 2>/dev/null | openssl dgst -sha256 -hex | sed 's/^.* //'
```

Step 3: Get API Server Advertise address

```
$ kubectl cluster-info
Kubernetes control plane is running at https://<HOST>:<PORT>
KubeDNS is running at https://<HOST>:<PORT>/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```

Setp 4: Join a new Kubernetes Worker Node a Cluster

```
$ kubeadm join <control-plane-host>:<control-plane-port> --token <token> --discovery-token-ca-cert-hash sha256:<hash>
```

## How to removing a worker node from the Cluster

```
$ kubectl drain  <node-name> --delete-local-data --ignore-daemonsets
$ kubectl cordon <node-name>
$ sudo kubeadm reset
```
