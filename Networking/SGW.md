## Storage Gateway
- Hybrid storage appliance (on-premise)
- Dùng để thêm storage từ on-premise qua AWS:
    + File Storage
    + Volume Storage
- Volume storage có thể được backups vào AWS
- Tape backups vào AWS

Có 3 mode:
1. TAPE Gateway (VTL) Mode
    - Tạo virtual tape để lưu vào S3 và Glacier
    - Kết nối AWS qua HTTPS
    - Được lưu vào S3 và nếu được archived thì chuyển qua Glacier
2. File Mode - dùng SMB và NFS
    - File Storage được backup tại S3 Objects
    - Files được lưu để share bằng NFS hoặc SMB 
    - Files được kết nối với AWS qua HTTPS (public endpoint)
    - Và được lưu 1:1 thành S3 Objects
    - SMB có thể tích hợp với active directory và file authorisation
3. Volume Mode:
    - Gateway Cache/Sotred tại S3 và EBS
    - Gateway Stored:
        + Có local storage và Upload Buffer
        + Có thể tới 16TB mỗi volume và max 32 volume, tổng cộng 512TB 
        + Dữ liệu chính được lưu tại on-premise, backup được lưu tại AWS **asynchronously**
        + Kết nối với APp Server qua iSCSi và kết nối với AWS qua HTTPS
        + Có thể dùng cho Disaster Recovery
        + AWS tạo EBS Snapshots từ dữ liệu backup
    - Gateway Cached:
        + Giống với Stored nhưng khác là dữ liệu chính được lưu tại AWS. Dữ liệu được cập nhật thường xuyên sẽ được cached locally. 
        + AWS lưu vào S3 và snapshots vào EBS
        + 32TB mỗi volume, 32 Volume MAX, tổng cộng 1PB 

