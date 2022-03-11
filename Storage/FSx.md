## FSx

### FSx for Windows
Dịch vụ quản lý files servers/shares (tập tin) cho Windows Servers. 

Được thiết kế để kết hợp với window environment 

Có thể dùng trong một hoặc nhiều AZ trong VPC

Có thể có backup khi cần (on-demand) hoặc có kế hoạch (on-scheduled)

Có thể truy cập qua VPC Peering, VPN, DX...

Có thể truy cập dùng SMB 

Cần phải có Directory


- VSS - User-Driven Restores
- Dùng Windows permission model 
- Hỗ trợ DFD - dùng để scale out file share structure 


### FSx for Lustre

Dùng cho High Performance Compute (HPC) - LINUX

Thường dùng cho Machine Learning, Big Data, Financial Modelling 

100 GB/s throughput 

Loại triển khai:
 - Scratch 
    + Dành cho ngắn hạn, nhanh
 - Persistent