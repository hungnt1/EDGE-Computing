## Khởi tạo Deployment trong KubeEdge

- Khởi tọa file deployment. Thực hiện chỉ định node chính là edge host vừa tạo

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      nodeName: ingress.novalocal
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
          hostPort: 80
```

- Khởi tạo deployment
```
kubectl create -f deployment.yaml

```

- Xem danh sách Pod
```
kubectl get pod  | grep nginx
nginx-deployment-59b7775946-sfz5k      1/1     Running     0          69m

```

- Xem describe pod
```
Name:         nginx-deployment-59b7775946-sfz5k
Namespace:    default
Priority:     0
Node:         ingress.novalocal/10.10.204.60
Start Time:   Tue, 14 Jul 2020 09:55:02 +0700
Labels:       app=nginx
              pod-template-hash=59b7775946
Annotations:  <none>
Status:       Running
IP:           172.17.0.4
IPs:
  IP:           172.17.0.4
Controlled By:  ReplicaSet/nginx-deployment-59b7775946
Containers:
  nginx:
    Container ID:   docker://a8ae6bb075ba566770b171a75fd6a44677e6612975a91fdd5cbb9f1f4fb3e13b
    Image:          nginx
    Image ID:       docker-pullable://nginx@sha256:a93c8a0b0974c967aebe868a186e5c205f4d3bcb5423a56559f2f9599074bbcd
    Port:           80/TCP
    Host Port:      80/TCP
    State:          Running
      Started:      Tue, 14 Jul 2020 09:55:20 +0700
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-sph8h (ro)
Conditions:
  Type           Status
  Initialized    True
  Ready          True
  PodScheduled   True
Volumes:
  default-token-sph8h:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-sph8h
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:          <none>

```

- Truy cập vào địa chỉ của Edge Host, nếu trả về default của Nginx có nghĩa quá trình setup thành công
![](https://i.imgur.com/YbbMXbQ.png)