## Step 2: Install kubelet, kubeadm and kubectl

```
$ sudo apt update
$ sudo apt -y install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common \
    net-tools \
    nfs-common \
    telnet

$ curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

$ cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
```
To install a latest version
```
$ sudo apt update

$ sudo apt install -y kubelet kubeadm kubectl

$ sudo apt-mark hold kubelet kubeadm kubectl
```

To install a specific version, list the available versions in the repo, then select and install:
```
$ apt-cache madison kubelet
$ export KUBEVERSION=<VERSION>
$ sudo apt install -y kubelet=$KUBEVERSION kubeadm=$KUBEVERSION kubectl=$KUBEVERSION
$ sudo apt-mark hold kubelet kubeadm kubectl
```