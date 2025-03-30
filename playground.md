KillerCoda

1. https://killercoda.com/
2. https://www.emailtick.com/

or

https://labs.play-with-k8s.com/

```
#hurnsodmocdecnjqcp@nbmbb.com
kubeadm init --apiserver-advertise-address $(hostname -i) --pod-network-cidr 10.5.0.0/16
rpm -ivh https://github.com/derailed/k9s/releases/download/v0.32.5/k9s_linux_amd64.rpm
curl -sk https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | (export VERIFY_CHECKSUM=false; sh)
kubectl apply -f https://raw.githubusercontent.com/cloudnativelabs/kube-router/master/daemonset/kubeadm-kuberouter.yaml
kubeadm token create --print-join-command

```

Deploy nginx to test
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/website/master/content/en/examples/application/nginx-app.yaml

```
or
```
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: "n"
  name: "n"
spec:
  containers:
  - command:
    - sleep
    - inf
    image: node
    name: "n"
    resources: {}
    volumeMounts:
    - mountPath: /myapp
      name: host-volumes 
      readOnly: false	  
  volumes:
  - name: host-volumes
    hostPath:
      path: /root/myapp
EOF

```
