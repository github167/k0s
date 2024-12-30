Create system:masters user in k0s (1)
```
openssl genrsa -out user1.key 2048
openssl req -new -key user1.key -out user1.csr  -subj "/O=system:masters/CN=user1"

openssl x509 -req -in user1.csr -CA /var/lib/k0s/pki/ca.crt -CAkey /var/lib/k0s/pki/ca.key -CAcreateserial -out user1.crt -days 500
#openssl x509 --text --in user1.crt |grep "Subject: "

kubectl config set-credentials user1 --client-certificate=/root/user1.crt --client-key=user1.key
kubectl config set-context user1-context --cluster=local --namespace=default --user=user1
kubectl config use-context user1-context

```

Create normal user in k0s (1)

```
openssl genrsa -out user1.key 2048
openssl req -new -key user1.key -out user1.csr  -subj "/CN=user1"

openssl x509 -req -in user1.csr -CA /var/lib/k0s/pki/ca.crt -CAkey /var/lib/k0s/pki/ca.key -CAcreateserial -out user1.crt -days 500

kubectl config set-credentials user1 --client-certificate=/root/user1.crt --client-key=user1.key
kubectl config set-context user1-context --cluster=local --namespace=default --user=user1
kubectl config use-context user1-context

cat << EOF | k0s kc apply -f -
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  namespace: default
  name: user1-role
rules:
- apiGroups: ["", "extensions", "apps"]
  resources: ["deployments", "pods", "services"]
  verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
EOF

cat << EOF | k0s kc apply -f -
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: user1-rolebinding
  namespace: default
subjects:
- kind: User
  name: user1
  apiGroup: ""
roleRef:
  kind: Role
  name: user1-role
  apiGroup: ""
EOF

```

Create user in k0s (2)
```
openssl genpkey -out john.key -algorithm Ed25519
openssl req -new -key john.key -out john.csr -subj "/CN=john"

cat <<EOF | kubectl apply -f -
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: john
spec:
  request: $(cat john.csr | base64 | tr -d "\n")
  signerName: kubernetes.io/kube-apiserver-client
  expirationSeconds: 86400  # one day
  usages:
  - client auth
EOF

kubectl certificate approve john
kubectl get csr/john -o jsonpath="{.status.certificate}" | base64 -d > john.crt
kubectl delete csr john
kubectl config set-credentials john --client-certificate=/root/john.crt --client-key=john.key
kubectl config set-context john-context --cluster=local --namespace=default --user=john
#kubectl create clusterrolebinding john-view --clusterrole=view --user=john
kubectl create clusterrole pod-manager --verb=create,list,get,delete --resource=pods
kubectl create clusterrolebinding c-john-pod-manager --clusterrole=pod-manager --user=john
kubectl auth can-i create deployments --namespace=default --as=john
kubectl auth can-i create secrets --namespace=default --as=john
kubectl auth can-i get pods --namespace=default --as=john
'''
