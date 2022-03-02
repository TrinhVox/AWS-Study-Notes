## Elastic Container Service (ECS)

ECS là dịch vụ để chạy containers trên cơ sở hạ tầng được quản lý bởi AWS. 

ECS có hai chế độ:

1. Dùng EC2 để chạy containers bằng ECS
2. Dùng fargate, là chế độ hoạt động serverless

ECS tạo ra cluster (nhóm/cụm) để người dùng chạy containers bên trong. 

Người dùng tạo một Container Definition để cho ECS biết DockerImage của người dùng ở đâu, port nào được dùng để kết nối. 

Người dùng tạo thêm một Task Definition để cho ECS biết các resource (tài nguyên) để chạy một container, Task Role được dùng để trao quyền cho container truy cập các resource của AWS. 

Người dùng viết Service Definition để cho ECS biết người dùng muốn tạo bao nhiêu containers, liên quan tới scaling. 

### ECS Cluster Types

#### EC2 Mode
- EC2 mode tạo ra các máy ảo EC2 trong tài khoản của người dùng để triển khai các tasks (công việc) và service (dịch vụ)
- Người dùng phải trả phí cho các máy ảo được tạo ra bất kể dùng bao nhiêu containers

#### Fargate mode
- Fargate mode dùng các cơ sở hạ tầng của AWS, và ENI's được để ở trong VPC của người dùng và có IP address ở trong VPC. 