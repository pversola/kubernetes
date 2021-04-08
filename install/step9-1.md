## Step 9: Install Dashboard

Install Helm

```
$ curl -fsSL https://baltocdn.com/helm/signing.asc | sudo apt-key add -
$ cat <<EOF | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
deb https://baltocdn.com/helm/stable/debian/ all main
EOF
$ sudo apt update
$ sudo apt install -y helm
```

Install Dashboard

```
$ helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/
$ helm install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard

$ cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kube-system
EOF
```
