## Step 9: Install Dashboard

```
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.1.0/aio/deploy/recommended.yaml

$ cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
---
apiVersion: rbac.authorization.k8s.io/v1
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
  namespace: kubernetes-dashboard
EOF

$ echo "" ; kubectl get secret -n kubernetes-dashboard $(kubectl get serviceaccount admin-user -n kubernetes-dashboard -o jsonpath="{.secrets[0].name}") -o jsonpath="{.data.token}" | base64 --decode ; echo ""

```


```
apiVersion: v1
kind: Service
metadata:
  labels:
    app: conn-gw
  name: conn-gw
  namespace: ebts
spec:
  clusterIP: 10.233.11.102
  ports:
    - name: http
      nodePort: 32103
      port: 80
      protocol: TCP
      targetPort: 80
  selector:
    app: conn-gw
  sessionAffinity: None
  type: NodePort
status:
  loadBalancer: {}
```
