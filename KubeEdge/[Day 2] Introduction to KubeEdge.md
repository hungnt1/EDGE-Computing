
[Day 2] Introduction to KubeEdge


- KubeEdge là một dự án OpenSource, được sử dụng để mở rộng các ứng dụng được đóng trong container và quản lý các device trên các Edge Host. Dự án được phát triển từ Kubernetes and cung cấp khả năng quản lý network, triển khai ứng dụng, đồng bộ metadata giữa  cloud và edge. Nó hỗ trợ MQTT và cho phép nhà phát triển custom các flow, và giao tiếp các tài nguyên device trên các Edge. KubeEdge bao gồm Cloud và Edge

## 1. Lợi ích
- Edge computing
    - By running business logic on Edge, a larger amount of data can be protected and processed where the data is generated. Edge nodes can run autonomously, effectively reducing the network bandwidth requirements and consumption between Edge and Cloud. By processing data through Edge, the response speed is significantly improved, and data privacy is protected.

- Simplified development
    - Developers can write regular http or mqtt-based applications, containerize them, and run them anywhere-whether in Edge or in the cloud-in a more suitable way.

- Support for native Kubernetes
    - using KubeEdge, users can coordinate applications on Edge nodes, manage devices and monitor applications and device status just like in traditional Kubernetes clusters. The location of the edge node is transparent to the customer.

- Rich applications
    - It is easy to deploy existing complex machine learning, image recognition, event processing and other advanced applications to Edge.


## 2. Tổng quan kiến trúc

- KubeEdge bao gồm 2 thành phần: Cloud và Edge

- Cloud Part
    - CloudHub: A WebSocket server, responsible for monitoring the status of the cloud and sending messages to EdgeHub.
    - EdgeController: Extended kubernetes controller for managing edge nodes and pod metadata in order to locate data to specific edge nodes.
- Edge Part
    - EdgeHub: A WebSocket client responsible for edge computing interaction with Cloud Service (such as Edge Controller in KubeEdge architecture). This includes synchronously updating cloud resources to the edge, and reporting the status of edge hosts and devices to the cloud.
    - Edged: An agent that runs on an edge node and manages containerized applications.
    - EventBus: The MQTT client interacts with the MQTT server to provide publish and subscribe functions for other components.
    - DeviceTwin: Responsible for storing device status and syncing device status to the cloud. It also provides a query interface for applications.
    - MetaManager: responsible for the messages between edged and edgehub. Also responsible for storing/retrieving metadata in a lightweight database (SQLite).


## 3. Kiến trúc

![](https://ithelp.ithome.com.tw/upload/images/20190916/20121071HLKXvHZftD.png)