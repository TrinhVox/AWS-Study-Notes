## Lambda

Lambda là Function-as-a-Service. 

Lambda là dịch vụ điện toán phi máy chủ. 

Khi tạo ra một lambda function người dùng cần chỉ định:
- Ngôn ngữ lập trình
- Deployment Package (Gói triển khai) - 50 MB Zipped/250 MB Unzipped
- Người dùng trực tiếp điều khiển bộ nhớ được chỉ định cho Lambda Functions tuy nhiên vCPU sẽ được chỉ định gián tiếp.
- Runtime có 512MB storage 
- Thời gian timeout của lambda là **900s/15 phút**

### Networking

#### Public Lambda

Theo mặc định, lambda functions được dùng public networking (mạng public). Lambda functions có thể truy cập các dịch vụ public của AWS và Internet. 

Tuy nhiên lambda fucntions sẽ không truy cập được VPC trừ khi có public IPs và được cho phép truy cập từ bên ngoài. 

#### Private Lambda

Khi lambda được chạy trong VPC, lambda functions phải tuân thủ theo các quy tắc mạng được đặt ra trong VPC. 


### Security

Để Runtime Environment trong mỗi lambda functions có thể truy cập tới các resources của AWS, runtime environment cần có Execution Role 

### Logging 

Logs từ Lambda - CloudWatch Logs => need permissions via Execution role

Metrics (success/failure, retries, latency) - CloudWatch

Distributed Tracing - X-Ray

### Invocation 

#### Synchronous Invocation 
Lamba function được khởi động bằng cách trực tiếp qua CLI/API và Lambda functions trả lại dữ liệu hoặc fails

#### Asynchronous Invocation
Lambda function được khởi động bằng các dịch vụ AWS (vd. S3...). Dịch vụ sẽ không chờ để lambda function trả lời. Lambda phải tự động chạy lại (reprocessing) từ 0-2 lần nếu bị thất bại.

Nếu events tiếp tục thất bại, sẽ được gửi tới **Dead Letter Queues**. Nếu events thành công hoặc thất bại, lambda hỗ trợ các điểm đến như - SQS, SNS, Lambda và EventBridge.

#### Event Source Mapping
Được dùng để process streams hoặc queues từ Kinesis, DynamoDB, SQS. 


### Versions 

Mỗi version (phiên bản) của lambda function là bao gồm code + cấu hình (configuration) của function đó.

Có thể dùng ALIAS để chỉ tới các versions khác nhau. 

