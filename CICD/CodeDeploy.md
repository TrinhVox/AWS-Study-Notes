## CodeDeploy

Code Deployment as a service

Dùng để deploy (triển khai) code chứ không phải resources (tài nguyên)

Có thể dùng CodeDeploy để triển khai code trên EC2, On-premises, Lambda Functions và ECS

Dùng CodeDeploy Agent ở On-premises hoặc EC2 để deploy code

### Appspec

- Dùng để quản lý deployment - dùng config (cấu hình) + lifecycle event hooks
- Có ba phần trong Appspec:
    + Files - dùng cho EC2/On-premises để cấu hình cần install gì trong lúc deploy 
    + Resources - dùng cho ECS/Lambda để chỉ định Lambda function (alias) hoặc route được chỉ định tới ECS containers
    + Permissions - dùng cho EC2/On-premises để chỉ định các permissions đặc biệt cho các files được deploy tại target
- Lifecycle event hooks:
    - ApplicationStop - event xảy ra trước khi ứng dụng được download - Có thể dùng event này để dừng ứng dụng hoặc xoá các packages đã được install trước cho lần deployment tiếp theo.
    - DownloadBundle - event để download ứng dụng đến một chỗ temporary (tạm thời)
    - BeforeInstall - event cho preinstall tasks như giải mã (decrypt) files và tạo backup cho version hiện tại
    - Install - trong event này CodeDeploy agent copy file đã được lưu tại folder tạm thời vào folder chính để deploy. 
    - AfterInstall - Dùng event này để cấu hình ứng dụng hoặc thay đổi permissions cho file
    - ApplicationStart - event này được dùng để restart các dịch vụ bị dừng tại ApplicationStop
    - ValidateService - dùng để check nếu deploy có thành công hay không

