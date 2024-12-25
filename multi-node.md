# k0s install
Installation url https://docs.k0sproject.io/stable/install/

Download
```
export http_proxy='http://example.com'
export https_proxy='http://example.com'
export no_proxy="localhost,127.0.0.1,172.30.236.179"
curl --proto '=https' --tlsv1.2 -sSf https://get.k0s.sh | sh

mkdir -p /etc/systemd/system/k0scontroller.service.d
cat > /etc/systemd/system/k0scontroller.service.d/http-proxy.conf << EOF
[Service]
Environment=CONTAINERD_HTTPS_PROXY='http://example.com'
EOF
```
Install Controller in server1
```
mkdir -p /etc/k0s
k0s config create > /etc/k0s/k0s.yaml
k0s install controller -c /etc/k0s/k0s.yaml
k0s start
k0s status
journalctl -u k0controller

k0s token create --role=worker #copy to clipboard

```

Install Worker in server2
```
# create tokenfile by copying the token string from controller
# vi tokenfile, paste the token string
k0s install worker --token-file /root/tokenfile
k0s start
k0s status
journalctl -u k0sworker
```

Create pod for testing
```
k0s kc get pod --all-namespaces
cat << EOF > deploy.yaml
kind: Service
apiVersion: v1
metadata:
  name: bootcamp-svc
  namespace: default
  labels:
    app: bootcamp
spec:
  ports:
    - protocol: TCP
      port: 8080
      nodePort: 30255
      targetPort: 8080
  type: NodePort
  selector:
    app: bootcamp
---

apiVersion: apps/v1
kind: Deployment
metadata:
  namespace: default
  name: bootcamp
  annotations: {}
  labels:
    app: bootcamp 
spec:
  selector:
    matchLabels:
      app: bootcamp
  replicas: 1
  template:
    metadata:
      labels:
        app: bootcamp
    spec:
      containers:    
        - name: container
          image: >-
            docker.io/jocatalin/kubernetes-bootcamp:v1
          #command: ["printenv"]
          ports:
            - containerPort: 8080
              protocol: TCP
          env:
            - name: MY_POD_NAME
              valueFrom:
                fieldRef:
                  fieldPath: metadata.name
            - name: MY_CPU_REQUEST
              valueFrom:
                resourceFieldRef:
                  containerName: container
                  resource: requests.cpu
            - name: TZ
              value: Asia/Hong_Kong
          volumeMounts:
            - name: podinfo
              mountPath: /tmp/podinfo
            - name: test-volume
              mountPath: /tmp/hostpath

      volumes:      
        - name: podinfo
          downwardAPI:
            items:
              - path: labels
                fieldRef:
                  apiVersion: v1
                  fieldPath: metadata.labels
              - path: annotations
                fieldRef:
                  apiVersion: v1
                  fieldPath: metadata.annotations
            defaultMode: 420
        - name: test-volume
          hostPath:
            path: /root
            type: DirectoryOrCreate			
      imagePullSecrets: []
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
  paused: false
EOF

k0s kc apply -f deploy.yaml


#k0s kc create deployment bootcamp --image=docker.io/jocatalin/kubernetes-bootcamp:v1 --port=8080
#k0s kc create deployment bootcamp --image=containous/whoami --port=8080
#k0s kc create deployment bootcamp --image=busybox --port=8080 -- sleep 3600
#k0s kc expose deployment/bootcamp --type="NodePort" --port=8080
#k0s kc patch svc/bootcamp --type='json' --patch='[{"op":"replace", "path":"/spec/ports/0/nodePort", "value":32055}]'

curl localhost:32055
```
