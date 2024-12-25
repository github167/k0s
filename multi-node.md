# k0s install

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

