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
Install single-node
```
k0s install controller --single
k0s start
k0s status
k0s kubectl get nodes
```

Uninstall single-node
```
k0s stop
k0s reset
```

Install Controller in server1



