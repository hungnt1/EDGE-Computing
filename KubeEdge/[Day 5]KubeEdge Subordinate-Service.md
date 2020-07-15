

- Bình thường trong K8S, các Pod không được public trực tiếp mà phải giao tiếp với nhau qua Service. Tuy nhiên network trong KubeEdge sẽ khác


- Một service thường có 2 type sau đây

1. Cluster IP
- Virtual IP cho phép các Pod truy cập
- Địa chỉ intranet trong cụm, ngoài cụm không thể truy cập

2. NodePort
- Sử dụng port để forword request sang service
- Mở cổng cho phép truy cập vào Service trên tất cả các Node.
- Từ ngoài có thể truy cập ngoài vào bằng Node IP + Port


- Thử khởi tạo Service trên Cluster
```
apiVersion: v1
kind: Service
metadata:
  name: mysvc
spec:
  selector:
    app: nginx
  ports:
  - port: 80
    targetPort: 80

```

- Xem sanh sách svc
```
kubectl get svc | grep mysvc

mysvc                 ClusterIP   10.103.6.221     <none>        80/TCP           32s

```
- Xem endpoint của svc đang trỏ về pod trong deployment đã tạo ở day 4
```
kubectl describe svc mysvc
```

- Từ trong pod đã được tạo từ deployment đang ở trên Edge host, thực hiện curl vào địa chỉ ClusterIP
```
apt-get update && apt-get install curl -y
 curl 10.103.6.221

```
- Hoàn toàn không thể ping được vào cluster IP do Pod trên Edge Host sử dụng, do IP và cổng trên Pod đã sử dụng bridge trực tiếp trên host.