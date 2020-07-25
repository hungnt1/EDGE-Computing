

## 1. Môi trường

- Trước khi cài đặt cần có một K8S Cluster đang hoạt động. KubeEdge Cloud sẽ được cài trên các máy chủ K8S API Server
Cloud installation 
|-- k8s / kubeadm / kubectl / kubelet
|-- Docker
|-- OpenSSL

- Đồng thời yêu cầu 1 host ngoài cluster để triển khai làm Edge Host

- Edge installation 
|-- Docker
|-- OpenSSL


## 2. Cài đặt trên Kube Cloud

- Thực hiện copy cấu hình
```
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config
```

- Thực hiện tải về keadm và boostrap KubeEdge CloudCore.advertise-address tương ứng với địa chỉ API cho các Edge Host kết nối đến.
```
wget https://github.com/kubeedge/kubeedge/releases/download/v1.3.1/keadm-v1.3.1-linux-amd64.tar.gz
cd keadm-v1.3.1-linux-amd64
cd keadm
./keadm init --advertise-address=10.10.204.58 
```

- Sau khi boostrap thành công, Kube Cloud sẽ trả về đoạn mã token để cho các Edge Host join vào 
```
./keadm join --cloudcore-ipport=10.10.204.58:10000 --token=038fe72b0a242f67c8a8ec6cf24844a86844d562bf69434fc62d879b0578240e.eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE1OTQ3MjAxOTJ9.AglB36uB2_ItTe-lJjfTenbgAmoYqlerQthvnEdhfTo

```

## 3. Cài đặt trên Kube Edge hay Edge Host

- Cài đặt Docker
```
# Install Docker CE
## Set up the repository:
### Install packages to allow apt to use a repository over HTTPS
apt-get update && apt-get install apt-transport-https ca-certificates curl software-properties-common

### Add Docker’s official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -

### Add Docker apt repository.
add-apt-repository \
  "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) \
  stable"

## Install Docker CE.
apt-get update && apt-get install docker-ce=18.06.2~ce~3-0~ubuntu

# Setup daemon.
cat > /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=cgroupfs"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF

mkdir -p /etc/systemd/system/docker.service.d

# Restart docker.
systemctl daemon-reload
systemctl restart docker
```


- Thực hiện tải về keadm và boostrap KubeEdge CloudCore.advertise-address tương ứng với địa chỉ API cho các Edge Host kết nối đến.
```
wget https://github.com/kubeedge/kubeedge/releases/download/v1.3.1/keadm-v1.3.1-linux-amd64.tar.gz
cd keadm-v1.3.1-linux-amd64
cd keadm
```

- Sau khi truy cập thư mục thành công, trên node Edge thực hiện join vào cluster
```
./keadm join --cloudcore-ipport=10.10.204.58:10000 --token=038fe72b0a242f67c8a8ec6cf24844a86844d562bf69434fc62d879b0578240e.eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE1OTQ3MjAxOTJ9.AglB36uB2_ItTe-lJjfTenbgAmoYqlerQthvnEdhfTo

```

- Xem danh sách node
```
 kubectl get nodes
NAME                STATUS   ROLES        AGE   VERSION
ingress.novalocal   Ready    agent,edge   17h   v1.17.1-kubeedge-v1.3.1
worker1.novalocal   Ready    master       13d   v1.18.5

```
