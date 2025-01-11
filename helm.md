helm chart hub: https://artifacthub.io/


[rabbit-mq](https://artifacthub.io/packages/helm/bitnami/rabbitmq)
```
########## rabbit-mq
helm repo add bitnami https://charts.bitnami.com/bitnami
helm upgrade --install my-rabbitmq bitnami/rabbitmq --version 15.2.0 --set persistence.enabled=false,service.type=NodePort

```

[jenkins](https://artifacthub.io/packages/helm/jenkinsci/jenkins)
```
helm repo add jenkinsci https://charts.jenkins.io/
helm upgrade --install my-jenkins jenkinsci/jenkins --version 5.7.21 --set persistence.enabled=false,controller.installPlugins=false,controller.agentListenerServiceType=NodePort
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
kubectl expose pod/my-pgadmin4-96fdcbd89-tvrn5 --type="NodePort"

```

[admin (db web admin)](https://artifacthub.io/packages/helm/cetic/adminer)
```
helm repo add cetic https://cetic.github.io/helm-charts
helm upgrade --install my-adminer cetic/adminer --version 0.2.1

```

[helm dashboard](https://artifacthub.io/packages/helm/komodor/helm-dashboard)
```
helm repo add komodor https://helm-charts.komodor.io
helm upgrade --install my-helm-dashboard komodor/helm-dashboard --version 2.0.3 --set dashboard.persistence.enabled=false
kubectl expose --type="NodePort" pod/my-helm-dashboard-9bdd5dc44-nh8br

```

[Kubernetes Dashboard](https://artifacthub.io/packages/helm/headlamp/headlamp)
```
helm repo add headlamp https://headlamp-k8s.github.io/headlamp/
helm upgrade --install my-headlamp headlamp/headlamp --version 0.27.0 --set persistentVolumeClaim.enabled=false,service.type=NodePort
kubectl expose --type="NodePort" pod/my-headlamp-7cb69f6cb7-97zh6
kubectl -n default create token my-headlamp

```

[juice-shop](https://artifacthub.io/packages/helm/securecodebox/juice-shop)
```
helm repo add securecodebox https://charts.securecodebox.io/
helm upgrade --install my-juice-shop securecodebox/juice-shop --version 4.11.0 --set service.type=NodePort,service.port=4321
# modify the livenessProbe.periodSeconds to 100 (same as readinessProbe) in deployment

```

[skooner - k8s GUI](https://artifacthub.io/packages/helm/christianhuth/skooner)
```
helm repo add christianhuth https://charts.christianhuth.de
helm upgrade --install my-skooner christianhuth/skooner --version 0.0.11 --set service.type=NodePort
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
helm upgrade --install my-bodgeit securecodebox/bodgeit --version 4.11.0 --set service.type=NodePort
## http://$NODE_IP:$NODE_PORT/bodgeit

```

[pacman](https://artifacthub.io/packages/helm/pacman/pacman)
```
helm repo add pacman https://shuguet.github.io/pacman/
helm upgrade --install my-pacman pacman/pacman --version 0.1.20 --set service.type=NodePort,mongodb.enabled=false

```

