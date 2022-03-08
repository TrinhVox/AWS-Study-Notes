## Elastic File System (EFS)

EFS là dịch vụ hệ thống tệp tin có thể được gắn với nhiều Linux EC2 Instance. 

Architecture:
- Được vận hành theo **NFSv4**
- EFS được dùng với nhiều Instances
- EFS là private service và theo mặc định là cô lập trong VPC EFS được tạo ra
- EFS được tạo ra trong VPC và được truy cập theo POSIX Permissions 
- EFS được truy cập bởi Mount Target được để trong mỗi AZ

+ EFS chỉ được sử dụng cho LINUX
+ Có hai mode:
    + General Purpose
    + MAX I/O
+ Có 2 throughput modes:
    + Bursting 
    + Provisioned
+ 2 Storage Class:
    + Standard
    + Infrequent Access
