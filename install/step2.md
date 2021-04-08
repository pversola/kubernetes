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

$ sudo apt update

$ sudo apt install -y kubelet kubeadm kubectl

$ sudo apt-mark hold kubelet kubeadm kubectl
```
