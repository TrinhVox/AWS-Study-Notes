## VPN

### Site-to-site

AWS Site-to-site VPN là một hardware VPN để tạo IPSEC VPN giữa AWS VPN và mạng ngoài như mạng tại công ty/mạng truyền thống tại chỗ. 

VPN được dùng để kết nối VPC và mạng tại chỗ một cách bảo mật bằng IPSEC, và được chạy trên Internet

### Architecture
Để tạo một site-to-site VPN thì cần chuẩn bị:
- IP Address Range của VPC cần kết nối với mạng on-premise
- IP Address Range của mạng on-premise và router trong mạng on-premise
- Đặt VGW tại VPC
- Đặt CGW tại mạng on-premise
- Kết nối VPN với VGW và CGW  
- Config (cấu hình) để route table chỉ đến đúng mạng on-premise 

=> Nếu CGW fails thì tất cả kết nối sẽ fail 

Nếu muốn thiết kế Fully HA Hybrid Environment thì cần:
- Kết nối thêm một CGW tại mạng on-premise (tốt nhất là ở một toà nhà khác)

### Virtual Private Gateway
Được đính kèm với VPC để nhận kết nối như các Gateway khác
- Highly Available
### Customer Gateway
Được đính kèm với mạng on-premise (trên router) để kết nối với mạng từ AWS 

**When to use VPN?"**

**Where to use VPN?**


## Static vs. Dynamic VPN 
Dynamic VPN được hỗ trợ bởi BGP - Border Gateway Protocol (protocol)

### Static
- Static VPN thêm static routes (đường truyền tĩnh) vào route table
- Mạng on-premise sẽ được cấu hình tính trên kết nối VPN 

### Dynamic 
BGP được config cho cả hai bên (AWS và on-premise)và networks sẽ được tự động trao đổi bởi BGP. 

**Considerations**
- Giới hạn tốc độ từ AWS - 1.25 Gbps (throughput) 
- Độ trễ - bởi vì được truyền qua mạng internet nên có thể có một độ trễ nhất định
- Phí - phí AWS được tính theo giờ, mỗi GB đầu ra, và giới hạn dữ liệu tại on-premise
- tốc độ để set-up
- Có thể dùng VPN là kết nối backup cho Direct Connect

### Advanced Site-to-Site VPN
VPN tunnels chỉ dùng để kết nối tới chỗ edge location gần nhất của global accelerator => độ trễ thấp hơn và throughput cao hơn

Tuy nhiên acceleration chỉ có thể được cho phép khi tạo với TGW VPN thôi, không thể dùng với VPN dùng VGW

Sẽ tính phí fixed công thêm data transfer


**CGW -> TGW -> VPC (n)**
TGW cho phép nhiều VPC truy cập 1 cặp VPN tunnels. 