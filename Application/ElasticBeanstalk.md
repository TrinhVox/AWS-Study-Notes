## Elastic Beanstalk

Platform as a Service - nghĩa là người dùng chỉ cần upload code và EB sẽ tự động chỉ định và cầu hình các resources. 

### Platforms 

EB có thể được dùng với:
- Các ngôn ngữ lập trình như Go, Python, Ruby, PHP, Node.js, .NET Core, .NET
- Containers như Docker & Multicontainer Docker

### Application
Application trong EB có nghĩa là tổng hợp các thành phần liên quan tới ứng dụng (application) - giống như một folder/container 

#### Application Versions 
Applications Versions là một version (phiên bản) của code đã được deploy (triển khai) của ứng dụng. Và source bundle sẽ được lưu tại S3. 

#### Environments 
Environments (Môi trường) trong application là containers cho các cơ sở hạ tầng và cấu hình cho một phiên bản của ứng dụng

Vd: Prod, test, processing 

Mỗi environment có thể là web server tier hoặc worker tier, tier sẽ điều khiển cấu trúc (structure) và chức năng (function) của environment. 
Mỗi tier sẽ có một ASG. 
- web server tier sẽ có một ELB để nhận traffic và sẽ chuyển traffic tới SQS queue. 
- SQS queue sẽ gửi các messages từ web server tier tới worker tier. 

Và các architecture và thành phần resources sẽ được quản lý bởi EB

Mỗi environment có thể có CNAME riêng. 

=> Databases phải được cấu hình ngoài Elastic Beanstalk - bởi nếu DB ở trong một ENV trong beanstalk mỗi khi đổi ENV người dùng sẽ mất dữ liệu trong DB 

### Deployment Policies

Deployment policies chỉ định cách các phiên bản (versions) của ứng dụng được deploy (triển khai) trong một environment (môi trường)

#### All at once 
Triển khai tất cả cùng một lúc, ứng dụng sẽ bị ngưng (outage) một thời gian ngắn
- nhanh và gọn, nhưng cách hồi phục không tốt 
- chỉ nên dùng cho testing 

#### Rolling
Triển khai theo từng batch (lô), người dùng có thể điều khiển nhiều hơn. Mỗi batch sẽ được lấy ra, đổi version, kiểm healthcheck trước khi để vào service. Nghĩa là ứng dụng chỉ tiếp tục được chạy khi healthcheck được clear 

#### Rolling with additional batch 
Khác với cách trước, thay vì batch được thay ra upgrade và để lại vào production, nghĩa là production vẫn sẽ được vận hành 100%. Trong lúc đó batch mới sẽ được kiểm tra. 

#### Immutable 
Tất cả instances được thay đổi với versions mới 

#### Traffic Splitting
Cho phép một số traffic được run với instances mới và một số với instances cũ 
=> Dùng cho A/B Testing 

### EB và RDS
Người dùng có thể tạo một RDS instance trong một EB environment (môi trường). Và Instance đó sẽ được kết nối với EB environment. 

Điều này nghĩa là nếu environment bị xoá thì RDS cũng sẽ bị xoá

Còn nếu người dụng chọn tạo một RDS instance riêng, không liên quan tới EB environment, thì người dùng phải tự tạo thêm các properties sau:
- RDS_HOSTNAME
- RDS_PORT
- RDS_DB_NAME
- RDS_USERNAME
- RDS_PASSWORD

Nếu muốn decouple (tách) RDS đã được tạo ra khỏi một EB Environment thì cần phải:
1. Tạo một RDS snapshot
2. "Enable Delete Protection"
3. Tạo một EB environment mới với cùng version của ứng dụng
4. Cho phép environment mới kết nối với RDS 
5. Swap environment bằng CNAME hoặc DNS
6. Xoá EB environment cũ 
7. Nhưng do "Enable Delete Protection" đã được chọn từ bước thứ 2, nên stack sẽ không thể xoá, chọn RDS để retain (giữ lại) và xoá stack 

### Customise
EB dùng cloudformation để tạo cơ sở hạ tầng cho mỗi environment. 

Nên để customise EB, thì người dùng có thể dùng Ebextensions trong cloudformation đẻ tạo thêm 

### EB Environment Cloning 

Tính năng cho phép người dùng tạo một environment mới giống như environment cũ một cách dễ dàng hơn. 

Vd: người dùng có thể dùng để copy PROD-ENV tới TEST-ENV mới để dành cho testing và QA

    => Các cấu hình đã được tạo trước cho PROD-ENV sẽ được giữ lại 

Clone sẽ copy RDS nhưng không copy data trong RDS 