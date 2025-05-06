# Create our own pod scheduler

Set kubectl proxy

```
kubectl proxy &

```

Create pod, with "schedulerName: my-scheduler"
```
kubectl delete deployment myapp
kubectl apply -f - << EOF
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: myapp
  name: myapp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      schedulerName: my-scheduler
      containers:
      - image: wangyanglinux/myapp:v1.0
        name: myapp	
EOF

```

Create my-scheduler.sh to bind the pod to arbitrary node

```

cat << "FINISH" > my-scheduler.sh
#!/bin/bash

SERVER='localhost:8001'

while true;
do
	for PODNAME in $(kubectl --server $SERVER get pods -o json | jq '.items[] | select(.spec.schedulerName == "my-scheduler") | select(.spec.nodeName == null) | .metadata.name' | tr -d '"');
	do
		NODES=($(kubectl --server $SERVER get nodes -o json | jq '.items[].metadata.name' | tr -d '"'))
		NUMNODES=${#NODES[@]}
		CHOSEN=${NODES[$[ $RANDOM % $NUMNODES]]}
		
		curl --header "Content-Type: application/json" --request POST --data @- http://$SERVER/api/v1/namespaces/default/pods/$PODNAME/binding/ << EOF
		{"apiVersion":"v1", "kind":"Binding","metadata":{"name":"$PODNAME"},"target":{"apiVersion":"v1","kind":"Node","name":"$CHOSEN"}}
EOF

		echo "Assigned $PODNAME to $CHOSEN"
	done
	sleep 1
done

FINISH
chmod u+x my-scheduler.sh
kubectl proxy &; sleep 2
./my-scheduler.sh
kill %"kubectl proxy &"

kubectl get deployment myapp

```
