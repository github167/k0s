[Install helm](https://helm.sh/docs/intro/install/)

```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```


helm chart hub: https://artifacthub.io/


[rabbit-mq](https://artifacthub.io/packages/helm/bitnami/rabbitmq)
```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm upgrade --install my-rabbitmq bitnami/rabbitmq --version 15.2.2 --set persistence.enabled=false,service.type=NodePort
echo "Username      : user"
echo "Password      : $(kubectl get secret --namespace default my-rabbitmq -o jsonpath="{.data.rabbitmq-password}" | base64 -d)"
echo "ErLang Cookie : $(kubectl get secret --namespace default my-rabbitmq -o jsonpath="{.data.rabbitmq-erlang-cookie}" | base64 -d)"

```

[jenkins](https://artifacthub.io/packages/helm/jenkinsci/jenkins)
```
helm repo add jenkinsci https://charts.jenkins.io/
helm upgrade --install my-jenkins jenkinsci/jenkins --version 5.8.3 --set persistence.enabled=false,controller.installPlugins=false,controller.serviceType=NodePort
#kubectl expose pod/my-jenkins-0 --type="NodePort"

```

[wordpress](https://artifacthub.io/packages/helm/bitnami/wordpress)
```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm upgrade --install my-wordpress bitnami/wordpress --version 24.1.4 --set persistence.enabled=false,mariadb.primary.persistence.enabled=false,service.type=NodePort,wordpressUsername=admin,wordpressPassword=admin

```

[postgresql admin](https://artifacthub.io/packages/helm/runix/pgadmin4)
```
helm repo add runix https://helm.runix.net/
helm upgrade --install my-pgadmin4 runix/pgadmin4 --version 1.33.3 --set persistentVolume.enabled=false,service.type=NodePort,env.email=admin@a.b,env.password=admin

```

[admin (db web admin)](https://artifacthub.io/packages/helm/cetic/adminer)
```
helm repo add cetic https://cetic.github.io/helm-charts
helm upgrade --install my-adminer cetic/adminer --version 0.2.1

```

[helm dashboard](https://artifacthub.io/packages/helm/komodor/helm-dashboard)
```
helm repo add komodor https://helm-charts.komodor.io
helm upgrade --install my-helm-dashboard komodor/helm-dashboard --version 2.0.3 --set dashboard.persistence.enabled=false,service.type=NodePort

```

[Kubernetes Dashboard](https://artifacthub.io/packages/helm/headlamp/headlamp)
```
helm repo add headlamp https://kubernetes-sigs.github.io/headlamp/
helm install my-headlamp headlamp/headlamp --version 0.35.0 --set persistentVolumeClaim.enabled=false,service.type=NodePort
kubectl -n default create token my-headlamp

```

[juice-shop](https://artifacthub.io/packages/helm/securecodebox/juice-shop)
```
helm repo add securecodebox https://charts.securecodebox.io/
helm upgrade --install my-juice-shop securecodebox/juice-shop --version 0.0.0 --set service.type=NodePort,service.port=4321
# modify the livenessProbe.periodSeconds to 100 (same as readinessProbe) in deployment

```

[skooner - k8s GUI](https://artifacthub.io/packages/helm/christianhuth/skooner)
```
helm repo add christianhuth https://charts.christianhuth.de
helm upgrade --install my-skooner christianhuth/skooner --version 0.0.12 --set service.type=NodePort
kubectl -n default create token my-skooner

```

[web site demo for pod usage](https://artifacthub.io/packages/helm/fairwinds-incubator/basic-demo)
```
helm repo add fairwinds-incubator https://charts.fairwinds.com/incubator
helm upgrade --install my-basic-demo fairwinds-incubator/basic-demo --version 1.0.0 --set hpa.enabled=false,service.type=NodePort

```
[simple plain web site demo for vulnerability](https://artifacthub.io/packages/helm/securecodebox/bodgeit)
```
helm repo add securecodebox https://charts.securecodebox.io/
helm upgrade --install my-bodgeit securecodebox/bodgeit --version 4.12.0 --set service.type=NodePort
## http://$NODE_IP:$NODE_PORT/bodgeit

```
[n8n](https://artifacthub.io/packages/helm/open-8gears/n8n)
```
helm upgrade --install my-n8n oci://8gears.container-registry.com/library/n8n --version 1.0.6 --set main.service.type=NodePort

```
