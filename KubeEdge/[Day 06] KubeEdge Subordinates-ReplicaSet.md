

- Tăng số sclae của deployment lên là 3
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
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
        image: nginx
        ports:
        - containerPort: 80
          hostPort: 80

```

- Apply
```
kubectl apply -f deployment.yaml
```

- Xem trạng thái deployment
```
 kubectl get deployment nginx-deployment
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   1/3     3            1           3h59m

```

- Kiểm tra dưới node Edge
```
Jul 14 13:54:20 ingress dockerd: time="2020-07-14T13:54:20.710098221+07:00" level=warning msg="Failed to allocate and map port 80-80: Bind for 0.0.0.0:80 failed: port is already allocated"

```
- Do hiện tại các pod trên Edge node sẽ sử dụng mạng cho container sẽ là Edge Node IP + Port. Cho nên hiện tại container có sẵn đã bind vào Port 80, cho nên việc replicate Pod là không thể.