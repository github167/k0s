install k9s
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
