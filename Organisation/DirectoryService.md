## AWS Directory Service 

Dịch vụ để quản lý directory (thư mục)

Chạy trong một VPC 

Phải triển khai tại nhiều AZs để có thể là HA

Một số dịch vụ của AWS cần phải có directory - vd: Amazon Workspaces

### Simple AD
- Lựa chọn rẻ nhất 
- Dùng Samba (giống Microsoft AD nhưng nhẹ hơn)
- có thể kết hợp với nhiều dịch vụ AWS
- Tới 500 users hoặc 5000 users
- Làm việc độc lập - không thể kết hợp với on-premise
### AWS Managed Microsoft AD
- Có thể kết hợp với on-premise qua VPN
- Dùng Microsoft AD
### AD Connector
- Khi chỉ cần dùng một dịch vụ mà cần phải có directory
- Kết nối với AD tại on-premise qua VPN 
- Chỉ là một identity chứ không phải AD riêng
- Không giữ chứa dữ liệu directory trên cloud
