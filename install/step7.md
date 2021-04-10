## Step 7: Add master node

Add master node

```
### COMMAND ###

$ sudo kubeadm join <ENDPOINT IP>:<PORT> --token <TOKEN> \
    --discovery-token-ca-cert-hash sha256:<DISCOVERY TOKEN> \
    --control-plane --certificate-key <CERTIFICATE KEY>
```

disabled scheduling from the cluster

```
$ kubectl taint nodes --all node-role.kubernetes.io/master-
```

Setup environment for master node

```
$ mkdir -p $HOME/.kube
$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
$ sudo chown $(id -u):$(id -g) $HOME/.kube/config
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

Step 4: Get Certificate Key

```
$ kubeadm certs certificate-key
```

Setp 5: Join a new Kubernetes Worker Node a Cluster

```
$ kubeadm join <control-plane-host>:<control-plane-port> --token <token> --discovery-token-ca-cert-hash sha256:<hash> --control-plane --certificate-key <CERTIFICATE KEY>
```
