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


AWS Region có **nhiều** Direct Connect Locations. Direct Connect Location được kết nối với AWS Region bằng redundant high speed connection (kết nối tốc độ cao). 

Trong mỗi DX location là nhiều AWS DX Router và router này được kết nối với AWS Region conceptually. Một khi chọn DX thì người dùng sẽ được thêm port của mình vào DX Router tại DX location. 

Người dùng cũng sẽ đặt Router riêng của mình (Customer hoặc provider DX Router) tại DX location. 

Theo mặc định, 1 cross-connect sẽ nối DX port với customer hoặc provider router. Và sẽ được extend kết nối từ DX location tới On-premises. 

## Resilience
Để thêm tính resilience cho DX, người dùng có thể dùng 2 DX ports trên 2 DX routers nối tới 2 Customer Routers khác nhau. 

Tốt hơn nếu người dùng chọn kết nối tới 2 DX location và 2 customer premises.
