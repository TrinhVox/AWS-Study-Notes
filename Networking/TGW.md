## Transit Gateway

- Network Transit Hub để kết nối VPCs tới mạng on-premise

- Advantage : giúp scale

Khi có nhiều VPCs thì CGW chỉ cần kết nối với TGW

VPC attachments được nối từ TGW tới một subnet ở mỗi AZ mà dịch vụ cần kết nối tới

TGW có thể kết nối nhiều khu vực với nhau và nhiều tài khoản. 

TGW có thể được kết nối với VPC, Site-to-Site VPN và DX Gateway 

TGW hỗ trợ transitive routing, có thể dùng để tạo global network 

