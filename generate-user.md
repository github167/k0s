Create system:masters user
```
openssl genrsa -out user1.key 2048
openssl req -new -key user1.key -out user1.csr -subj "/O=system:masters/CN=user1"

openssl x509 -req -in user1.csr -CA /var/lib/k0s/pki/ca.crt -CAkey /var/lib/k0s/pki/ca.key -CAcreateserial -out user1.crt -days 500
#openssl x509 --text --in user1.crt |grep "Subject: "

kubectl config set-credentials user1 --client-certificate=user1.crt --client-key=user1.key
kubectl config set-context user1-context --cluster=local --namespace=default --user=user1
kubectl config use-context user1-context

```

Create normal user and apply RBAC

```
openssl genrsa -out user2.key 2048
openssl req -new -key user2.key -out user2.csr -subj "/CN=user2"

openssl x509 -req -in user2.csr -CA /var/lib/k0s/pki/ca.crt -CAkey /var/lib/k0s/pki/ca.key -CAcreateserial -out user2.crt -days 500

cat << EOF | k0s kc apply -f -
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  namespace: default
  name: user2-role
rules:
- apiGroups: ["", "extensions", "apps"]
  resources: ["deployments", "pods", "services"]
  verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
EOF

cat << EOF | k0s kc apply -f -
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: user2-rolebinding
  namespace: default
subjects:
- kind: User
  name: user2
  apiGroup: ""
roleRef:
  kind: Role
  name: user2-role
  apiGroup: ""
EOF

kubectl config set-credentials user2 --client-certificate=user2.crt --client-key=user2.key
kubectl config set-context user2-context --cluster=local --namespace=default --user=user2
kubectl config use-context user2-context

```

Create x509 normal user
```
openssl genpkey -out user3.key -algorithm Ed25519
openssl req -new -key user3.key -out user3.csr -subj "/CN=user3"

cat <<EOF | kubectl apply -f -
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: user3
spec:
  request: $(cat user3.csr | base64 | tr -d "\n")
  signerName: kubernetes.io/kube-apiserver-client
  expirationSeconds: 86400  # one day
  usages:
  - client auth
EOF

kubectl certificate approve user3
kubectl get csr/user3 -o jsonpath="{.status.certificate}" | base64 -d > user3.crt
kubectl delete csr user3
kubectl config set-credentials user3 --client-certificate=user3.crt --client-key=user3.key
kubectl config set-context user3-context --cluster=local --namespace=default --user=user3
#kubectl create clusterrolebinding user3-view --clusterrole=view --user=user3
kubectl create clusterrole pod-manager --verb=create,list,get,delete --resource=pods
kubectl create clusterrolebinding c-user3-pod-manager --clusterrole=pod-manager --user=user3
kubectl auth can-i create deployments --namespace=default --as=user3
kubectl auth can-i create secrets --namespace=default --as=user3
kubectl auth can-i get pods --namespace=default --as=user3

'''
