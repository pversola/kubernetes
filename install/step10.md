## Step 10: Install metrics server

```
$ kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

**_FIX:_** Depending on your cluster setup, you may also need to change flags passed to the Metrics Server container. Most useful flags:

`--kubelet-preferred-address-types` - The priority of node address types used when determining an address for connecting to a particular node (default [Hostname, InternalDNS, InternalIP, ExternalDNS, ExternalIP])  
`--kubelet-insecure-tls` - Do not verify the CA of serving certificates presented by Kubelets. For testing purposes only.  
`--requestheader-client-ca-file` - Specify a root certificate bundle for verifying client certificates on incoming requests.

```
$ kubectl edit deploy -n kube-system metrics-server
[...]
spec:
  [...]
  template:
    [...]
    spec:
      containers:
      - args:
        - --cert-dir=/tmp
        - --secure-port=4443
        - --kubelet-insecure-tls
        - --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
        - --kubelet-use-node-status-port
        image: k8s.gcr.io/metrics-server/metrics-server:v0.4.2

[...]
```
