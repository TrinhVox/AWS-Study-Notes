## Snowball
Khi người dùng cần chuyển một số lượng lớn dữ liệu thì có thể đặt hàng Snowball thì AWS. 

- Snowball được mã hoá (at-rest) dùng KMS
- 50TB hoặc 80TB
- 1 Gbps hoặc 10Gbps Network 
- 10 TB tới 10PB thì sẽ tiết kiệm hơn
- Có thể đặt 10 Snowball và gửi tới 10 nơi khác nhau 

## Snowball Edge 

Giống với snowball nhưng thêm compute (điện toán)

Capacity (dung lương) lớn hơn snowball

- 10 Gbps, 10/25, 45/50/100 Gbps
- Storage Optimized bao gồm:
    + 80 TB
    + 24 vCPU
    + 32 Gib RAM
    + 1 TB SSD
- Compute Optimized bao gồm:
    + 100 TB
    + 52 vCPU
    + 208 GiB RAM
- Compute with GPU
=> Nên dùng khi cần làm việc ở xa hoặc xử lý dữ liệu lúc nhận dữ liệu

## Snowmobile 

Xe tải để lưu trữ dữ liệu 

- Cần khi 10PB+ 
- Có thể lên đến 100PB mỗi snowbile

## Data Sync
Dịch vụ thay vì là thiết bị chuyển dữ liệu 

Được thiết kế để chuyển dữ liệu lớn

Giữ metadata(vd: permissions (quyền), timestamps (thời gian))

Scalable - 10Gbps mỗi agent 

Có compression và encryption (mã hoá)

Có thể kết hợp với các dịch vụ khác của AWS (vd: S3)

Data Sync Agent cần được cấu hình tại on-premise

Dùng NFS hoặc SMB

