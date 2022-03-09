## Amazon Machine Image (AMI)

AMI được dùng để tạo EC2 instance. 

AMI có thể được cung cấp bởi cộng đồng hoặc AWS

AMI thuộc về khu vực, nghĩa là có nhiều AMI khác nhau có cùng công dụng nhưng được cung cấp ở những khu vực khác nhau. 

AMI có thể quản lý quyền truy cập, ví dụ quyền chỉ có riêng người dùng được truy cập, hoặc được truy cập công cộng. 

Có thể dùng Instance đang sử dụng để tạo template (mẫu) cho AMI

AMI baking có 4 bước:
1. Launch
- Dùng AMI để tạo Instance (máy ảo) 
2. Configure
- Dùng Instance đã được tạo để thay đổi cấu hình 
3. Create Image
- Dùng Instance với cấu hình đã được chỉnh để tạo AMI mới dựa trên cấu hình đó
- AMI sẽ gồm có:
    - Permission (quyền truy cập máy)
    - Snapshots của EBS được lưu vào AMI qua Block Device Mapping (Block Device Mapping được dùng để nối Snapshot với Block Device của Snapshot)
4. Launch
- AMI vừa được tạo sẽ được dùng để tạo Instance mới 


