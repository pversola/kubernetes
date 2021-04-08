Step 6: Install network plugin (CNI) configuration on Master

```
$ kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

Check cluster status: `$ kubectl cluster-info`  
Check all pods are running in kube-system: `watch kubectl get pods -n kube-system`  
Confirm master node is ready: `$ kubectl get nodes`

**_FIX:_** remove old calico configs from kubernetes without `kubeadm reset`

```
$ sudo ip route flush proto bird
$ sudo ip link list | grep cali | awk '{print $2}' | cut -c 1-15 | xargs -I {} ip link delete {}
$ sudo modprobe -r ipip
$ sudo rm /etc/cni/net.d/10-calico.conflist && rm /etc/cni/net.d/calico-kubeconfig
$ sudo service kubelet restart
```
