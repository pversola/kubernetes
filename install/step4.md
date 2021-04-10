## Step 4: Install Container runtime

```
$ cat <<EOF | sudo tee /etc/modules-load.d/containerd.conf
overlay
br_netfilter
EOF

$ sudo modprobe overlay
$ sudo modprobe br_netfilter

### Setup required sysctl params, these persist across reboots. ###
$ cat <<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf
net.bridge.bridge-nf-call-iptables  = 1
net.ipv4.ip_forward                 = 1
net.bridge.bridge-nf-call-ip6tables = 1
EOF

### Apply sysctl params without reboot ###
$ sudo sysctl --system
```

Install CRI-O

```
$ export OS=xUbuntu_20.04
$ export VERSION=1.20

$ cat <<EOF | sudo tee /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list
deb https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/ /
EOF

$ cat <<EOF | sudo tee /etc/apt/sources.list.d/devel:kubic:libcontainers:stable:cri-o:$VERSION.list
deb http://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable:/cri-o:/$VERSION/$OS/ /
EOF

$ curl -fsSL https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/Release.key | sudo apt-key --keyring /etc/apt/trusted.gpg.d/libcontainers.gpg add -

$ curl -fsSL https://download.opensuse.org/repositories/devel:kubic:libcontainers:stable:cri-o:$VERSION/$OS/Release.key | sudo apt-key --keyring /etc/apt/trusted.gpg.d/libcontainers-cri-o.gpg add -

$ sudo apt update
$ sudo apt -y install \
    cri-o \
    cri-o-runc
```

Start CRI-O:

```
$ sudo systemctl daemon-reload
$ sudo systemctl enable crio --now

$ cat <<EOF | sudo tee /etc/crio/crio.conf.d/02-cgroup-manager.conf
[crio.runtime]
conmon_cgroup = "pod"
cgroup_manager = "cgroupfs"

[crio.image]
registries = [
"quay.io",
"docker.io"
]
EOF

$ cat <<EOF | sudo tee /etc/default/kubelet
KUBELET_EXTRA_ARGS=--feature-gates="AllAlpha=false,RunAsGroup=true" --container-runtime=remote --container-runtime-endpoint='unix:///var/run/crio/crio.sock' --runtime-request-timeout=5m
EOF

$ sudo systemctl restart crio kubelet
$ sudo kubeadm config images pull
```
