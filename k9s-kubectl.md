install k9s
```
curl https://gist.githubusercontent.com/didhd/57fecbe3090c48272ea3997a54cd2330/raw/946cf17f89a7623da9893fb635e9e13d499f7d68/install_k9s.sh | bash

```
or
```
wget https://github.com/derailed/k9s/releases/download/v0.32.5/k9s_linux_amd64.deb
apt install ./k9s_linux_amd64.deb
k0s kubeconfig admin > ~/.kube/config

```
install kubectl
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

```
