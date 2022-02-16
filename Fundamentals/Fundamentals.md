# AWS Accounts - Tài khoản AWS

AWS Account is a *container* for **identities** (users) and **resources** (tài nguyên). 

Mỗi tài khoản AWS sẽ có một **root users** và được đăng ký bằng một tài khoản email độc nhất (unique).

Root user có thể truy cập tất cả tài nguyên được tạo bởi tài khoản này và không bị giới hạn (by default).

# Tạo tài khoản:
## Best Practice:
### Management Account (Tài khoản chính)
- MFA
- Billing Alarm
- IAM ADMIN
    Hạn chế dùng quyền lợi của root users bằng cách dùng IAM Admin.

### Production Account 
- MFA
- Billing Alarm
- IAM ADMIN
    Hạn chế dùng quyền lợi của root users bằng cách dùng IAM Admin.

## Multi-Factor Authentication (MFA)
MFA là tính năng được sử dụng để gia tăng bảo mật của tài khoản AWS. 
Khi user đăng nhập tài khoản, ngoài sử dụng tên đăng nhập và mật khẩu, user sẽ phải nhập mã OTP để xác nhận.

Khi tạo MFA cho users:
- AWS sẽ tạo Secret Key và thông tin khác (tên tài khoản, dịch vụ sử dụng,..) và dùng để tạo mã QR
- User dùng ứng dụng trên điện thoại để scan mã và mã OTP sẽ được tạo mỗi 10s

## IAM GROUP, USERS and ROLES Basics
Tất cả IAM identities sẽ được cho phép truy cập toàn bộ hoặc giới hạn truy cập vào tài nguyên được tạo ra bởi tài khoản AWS đang sử dụng.

IAM Identities khi mới được tạo sẽ không có quyền truy cập nào, và chỉ khi được cấp quyền mới được truy cập dịch vụ và tài nguyên đã tạo của tài khoản. 

- IAM is a globally resilience service (ie. All data is always secured across all AWS regions)
### IAM User
Identities which represent human or applications that need access to your account
Cá nhân hoặc ứng dụng cần truy cập tài khoản AWS đã được tạo ra. 
### IAM Group
Nhóm users có cùng công việc liên quan tới nhau
### IAM Roles
Dùng bởi AWS Services, hoặc để trao quyền truy cập cho users của tài khoản khác.

Công dụng chính của **IAM**:
- Quản lý các cá nhân được truy cập tài khoản AWS (ID Provider - IDP)
- Xác thực (Authenticate)
- Cho phép truy cập dịch vụ hoặc tài nguyên của tài khoản (Authorize)
### IAM Access Keys
Access Keys giúp AWS Command Line Tools (CLI Tools) tương tác với tài khoản AWS
Access Keys có thời hạn lâu dài
Mỗi IAM user có **hai** access keys:
- Access Key ID
- Secret Access Key
Access keys có thể được tạo ra, xoá đi hoặc bị tạm ngưng hoạt động
=> Một khi bị mất secret access key, thì sẽ không tìm lại được và sẽ phải tạo một bộ access keys mới và cập nhật CLI Tools với access keys mới. 

```
# check aws CLI version
aws --version

# configure credentials for CLI to interact with AWS Account 
aws configure --profile iamadmin-general
# enter the Access Keys ID and Secret Access Keys
```

# Budgets
Dùng AWS Budgets để tạo ngân sách cho tài khoản và nhận báo động khi tài khoản sử dụng lố ngân sách tạo ra.

---
# Cloud Computing
## NIST
- On-demand Self-Service 
- Broad Network Access
- Resource Pooling
- Rapid Elasticity - linh hoạt
- Measured Service

## Service Model
### IaaS - Cơ sở hạ tầng dưới dạng dịch vụ
IaaS cung cấp truy cập vào tính năng mạng, máy tính và không gian lưu trữ dữ liệu
### PaaS - Nền tảng dưới dạng dịch vụ 
PaaS cung cấp cơ sở hạ tầng và nguời dùng chỉ cần triển khai và quản lý các ứng dụng
### SaaS - Phần mềm dưới dạng dịch vụ
SaaS cung cấp phần mềm hoàn chỉnh đã được nhà cung cấp vận hành và quản lý
---
# Tech Fundamental

## Networking - Mạng
### OSI 7-Layer Model
**Media Layers**
1. Physical
    Physical layer có thể là đường truyền wifi để kết nối các thiết bị với nhau và trao đổi qua RAW BIT STREAMS.
    Layer này tất cả dữ liệu sẽ được đọc bởi tất cả thiết bị được kết nối => Thiết bị kết nối không được nhận dạng.
    Nếu hai thiết bị truyền dữ liệu cùng một lúc sẽ xảy ra va chạm và tất cả dữ liệu sẽ bị vô hiệu hoá. 
    Các thiết bị phải có cùng tiêu chuẩn để truyền thông tin với nhau mới có thể kết nối được. 
2. Data Link
    Ví dụ layer 2 là Ethernet (LAN - Local Area Network)
    Layer 2 dùng **frame** là đơn vị dữ liệu để trao đổi.
    Mỗi thiết bị ở layer 2 đều có một địa chỉ **MAC Address** (vd: 3e:22:fb:b9:5b:75) - hex address (48 bits)
    Có thể phát hiện va chạm ở đường truyền dữ liệu và hạn chế va chạm. 
3. Network
    Ví dụ protocol ở layer 3 là internet
    Layer 3 dùng Internet Protocol (IP) để kết nối các thiết bị. 
    IP dùng **IP address** (địa chỉ) để nhận dạng thiết bị và dùng routing (định tuyến) để truyền dữ liệu giữa các mạng khác nhau
    Đơn vị dữ liệu để trao đổi giữa các thiết bị là **IP Packets**
    **Routers** là thiết bị được dùng ở layer 3 để truyền các IP Packets giữa các hệ thống mạng khác nhau
    IP Packets có cấu trúc IPv4 hoặc IPv6, một số phần trong IP Packets gồm có:
    - Destination IP Address (Địa chỉ nơi đến)
    - Source IP Address (Địa chỉ nơi gửi)
    - Protocol (giao thức) - eg: TCP, ICMP, UDP
    - Data (Dữ liệu)
    Routers dùng routing table để tìm đường đi để gửi gói dữ liệu từ máy tính này sang máy tính khác.
    Routing dùng Address Resolution Protocol (ARP) để tìm MAC Address ở Layer 2 cho mỗi IP address ở Layer 3 để gửi frame dữ liệu 
**Host Layers**
4. Transport
    Transmission Control Protocol (TCP) và User Datagram Protocol (UDP)
    TCP sẽ chậm hơn nhưng đảm bảo gói dữ liệu sẽ được giao tới đúng đích đến. 
    UDP sẽ nhanh hơn nhưng không đảm bảo được gói dữ liệu sẽ được giao tới đúng đích đến
    TCP dùng đơn vị dữ liệu **Segment**
    Segment được để ở trong mỗi gói dữ liệu (IP Packets)
    Segment bao gồm:
    - Source port 
    - Destination port
    - Sequence number (số thứ tự)
    - Acknowledgement
    TCP Connection 3-way Handshake:
    - to synchronise and acknowledge sequence number and create a connection
5. Session
    Stateless Firewall: 
    - Có hai luật cần thiết là OUT và IN
    Stateful Firewall:
    - Chỉ có một hướng => Nếu đã cho kết nối một chiều thì chiều còn lại sẽ được kết nối
6. Presentation
7. Application

### Network Address Translation (NAT)
Để kết nối thiết bị riêng được kết nối với Internet thì NAT sẽ được dùng để dịch các địa chỉ IPv4 riêng thành địa chỉ công khai
- Static NAT
    dùng để chuyển 1 địa chỉ IP riêng thành 1 địa chỉ công khai ấn định (Internet GateWay)
- Dynamic NAT
    dùng để chuyển IP address nhưng không có 1 địa chỉ công khai ấn định mà sẽ dùng địa chỉ nào có sẵn vào thời điểm đó
- Port Address Translation - nhiều IP address riêng thành 1 địa chỉ công khai

### Subnetting
- Địa chỉ IPv4 được quyền route ở mạng riêng
    10.0.0.0 - 10.255.255.255 (16,777,216 địa chỉ)
    172.16.0.0 - 172.31.255.255 (16 x 65,536 địa chỉ)
    192.168.0.0 - 192.168.225.255 (256 x 256 địa chỉ)
- Prefix nằm sau IP address -> Số prefix càng lớn network càng nhỏ 




