## Direct Connect 

Là dịch vụ để kết nối trực tiếp với AWS và mạng on-premise 

DX là một port (cổng) được kết nối với AWS cho 1 Gbps hoặc 10 Gbps

Port được trao cho user tại DX Location:
- nếu là 1 Gbps thì phải theo tiêu chuẩn **1000-Base-LX** 
- nếu là 10 Gbps thì phải theo tiêu chuẩn 
**10GBASE-LR**

Và route tại mạng on-premise cần phải hỗ trợ:
- VLANS/BGP

Có thể có Multiple Virtual Interfaces (VIFS) trên một DX:
- Private VIF - kết nối với VPC (1 VIF = 1 VPC)
- Public VIF - kết nối với Public Zone Services
- VIFS = 1 VLAN & 1 BGP session tại bên client
DX không phải HA và không hỗ trợ Encryption bởi vì là Physical cable 

**Considerations**
- Cần nhiều thời gian để setup so với VPN
- Có thể dùng tới 40Gbps với Aggregation
- Không dùng internet nên độ trễ thấp
- Không có Encryption
