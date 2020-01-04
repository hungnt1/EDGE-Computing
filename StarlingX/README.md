

## 1. StarlingX Introduction

- StarlingX là một phần mềm cho phép triển khai các edge cloud với khả năng scale lên tới 100 server 
- Những key chính trong chức năng của StarlingX
    - Chung cấp dưới dạng một gói duy nhất bao gồm OS, storage, networking để phục vụ chạy một edge
    - tối ưu hóa phần mềm để phù hợp với môi trường edge
    - Được thiết với các với cấu hình sẵn cho nhiều môi trường edge đa dạng 
    - Dễ làm tương thích với các dự án opensource 
    - Cung cấp khả dụng cao cho người dùng
    - Cung cấp khả năng bảo mật trong giao tiếp, độ trễ thấp, thời gian uptime tăng, và các phân bổ các luồng steam.

## 2. Một số thuật ngữ trong StarlingX

- All-in-one Controller Node : một node vật lý cung cấp tất cả chức năng controller, compute và storage
- Bare Metal: một node vật lý mà không cài đặt hypervisor
- Compute hay worker : một node trong starlingx edge cloud được sử dụng để chạy một ứng dụng. Có thể lên tới 100 node trên một edge cloud. Chạy virtual switch và L3 routing
- Controller: sử dụng để điều khiển một starlingx edge cloud. 
- DataNetwork : Sử dụng để làm mạng internal giữa các VM. Chỉ compute node và all-in-one node mới yêu cầu một hoặc 2 interface cho Datanetwork
- Infra network: đã được loại bỏ 
- IPMI Network: được sử dụng để làm việc với IPMI của các node đã kết nối vào hệ thống từ node Controller.
- Management Network: đường mạng nội bộ giữa các node, được sử dụng để quản lý, giám sát và VM access I/O vào storage
- Node: một máy chủ nằm trong một server-class
- Node interface : một cổng mạng của máy chủ ( network card device)
- OAM network: đường mạng để StarlingX expose API
- PXEBoot Network: được sử dụng để boot OS  khác node khác ( PXE) qua network. thông thường controller sẽ sử dụng network để boot/install các node trong Openstack Cloud. Tất cả các node đều yêu cầu kết nối vào PXEBoot network. Đa số BIOS không hỗ trợ PXE thông qua đường mạng tagged VLAN, trong một số trường hợp PXE network nên ở đường no tag.
- Storage: một edge cloud được sử dụng để lưu trữ file và object của ứng dụng. Storage của StarlingX sử dụng CEPH, với khả năng  replication factor 
- Virtual Machines (VM): được phân bổ bởi hypervisor để chạy các ứng dụng.


## 3. Một số tùy chọn cho quá trình triển khai 
- All-in-one Simple: môt node thực hiện tất cả chức năng của một edge cloud ( control, storage, and application workloads). Tuy nhiên không có node HA cho edge 
- All-in-one Duplex: môt node thực hiện tất cả chức năng của một edge cloud ( control, storage, and application workloads). Tuy nhiên  có thêm một node HA cho edge 
- Standard with Controller Storage: hỗ trợ 1 hoặc 2 controller node đồng thời cung cấp storage cho cloud. Config này phù hợp cho cho edge cloud không yêu cầu nhiều về lưu trữ
- Standard with Dedicated Storage: cung cấp một node chuyên dụng cho lưu trữ ngoài controller và compute, với các edge cloud có nhu cầu lưu trữ cao.
- Standard with Ironic: cho phép cấu hình thêm Openstack Ironic service, cho phép ứng dụng chạy trên bare metal service.
-  Distributed Cloud ( chức năng đang phát triển): cho phép 