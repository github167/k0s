# Create PV from hostPath

Create pvc and pv
```
mkdir -p /mnt/data
echo "hello" > /mnt/data/index.html

kubectl apply -f - << EOF
apiVersion: v1
kind: PersistentVolume
metadata:
  name: task-pv-volume
  labels:
    type: local
spec:
  storageClassName: manual
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: task-pv-claim
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
EOF

kubectl wait --for=jsonpath='{.status.phase}'=Bound pvc/task-pv-claim
kubectl get pvc -o wide

```

Create a pod to test the PVC
```
kubectl delete pod task-pv-pod
kubectl apply -f - << EOF
apiVersion: v1
kind: Pod
metadata:
  name: task-pv-pod
spec:
  volumes:
    - name: task-pv-storage
      persistentVolumeClaim:
        claimName: task-pv-claim
  containers:
    - name: task-pv-container
      image: nginx
      ports:
        - containerPort: 80
          name: "http-server"
      volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: task-pv-storage
EOF

kubectl wait --for=condition=ready pod/task-pv-pod
kubectl get pod -o wide
curl $(kubectl get pod task-pv-pod --template={{.status.podIP}})

```
