KillerCoda

1. https://www.emailtick.com
2. https://killercoda.com
3. https://kodekloud.com/public-playgrounds
4. https://labs.play-with-k8s.com

```
#hurnsodmocdecnjqcp@nbmbb.com
kubeadm init --apiserver-advertise-address $(hostname -i) --pod-network-cidr 10.5.0.0/16
curl https://gist.githubusercontent.com/didhd/57fecbe3090c48272ea3997a54cd2330/raw/946cf17f89a7623da9893fb635e9e13d499f7d68/install_k9s.sh | bash
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
#k get pods
#k exec -it n -- /bin/bash

#apt udpate; apt install vim

```
