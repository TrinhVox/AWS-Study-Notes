## Network Interfaces

Mỗi máy EC2 sẽ có một ENI (Elastic Network Interfaces) chính gọi là primary ENI. Người dùng cũng có thể tạo thêm các ENI phụ thuộc (secondary ENI), các ENI này có thể ở một subnet khác. Nhưng các ENI đều sẽ ở chung 1 AZ. 

ENI gồm có:
- MAC Address
- Primary IPv4 Private IP (Địa chỉ IP riêng chính)
- Có thể có 0 hoặc nhiều hơn một secondary IPs
- Có thể có 0 hoặc 1 Public IPv4 Address
- 1 elastic IP mỗi private IPv4
- Security Group
- source/destination check 

Secondary ENI có thể được gỡ khỏi một instance và ghép với íntance khác

