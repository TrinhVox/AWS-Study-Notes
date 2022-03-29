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

Nếu Functions được publish thì version đó sẽ không thể thay đổi (immutable)
- Functions sẽ bị khoá và không thể edit version đã được publish
- Mỗi version sẽ có một unique ARN (ARN độc nhất) 
### Aliases
- Alias là một pointer tới function version
- Mỗi Alias có một unique ARN chỉ dành riêng cho alias đó 
- Tuy nhiên alias có thể được update và thay đổi function version mà alias đó chỉ tới 

Vd: 
- Alias PROD => bestanimal:1, BETA => bestanimal:2
- Sau thời gian test, người dùng muốn deploy bestanimal:2 ra production thì có thể dùng PROD để chỉ tới bestanimal:2
- Alias còn có tính năng Alias Routing để chỉ một phần trăm người dùng tới v1 còn phần trăm còn lại tới v2 => tính năng này có thể được dùng để test version mới 


### Handler
Có hai giai đoạn trong Lambda:
1. INIT
    Giai đoạn INIT gồm có những phần:
    - Function Init
    - Runtime Init
    - Extension Init
2. Shutdown
    Giai đoạn Shutdown gồm có:
    - Runtime shutdown
    - Extension shutdown

Phần Function initialisation (init) sẽ được thực hiện mỗi cold start

Sau lần đầu function được chạy thì các lần sau, mỗi invoke chỉ có phần handler chạy và lambda sẽ dùng environment đã tạo lần trước. 

### Environment Variables
Một Environment Variable gồm có một cặp key và value 

Environment variables cho phép code execution được thay đổi theo variables 

### Monitoring
Tất cả số liệu (metrics) đều có thể được truy cập qua CloudWatch

### Logging
Lambda Execution Logs sẽ được gửi qua CloudWatch Logs 

Log Groups = aws/lambda/functionname

Log Stream = YYYY/MM/DD[$LATEST||version]

Nếu để Lambda role ở chế độ mặc định (default), lambda sẽ có permissions (quyền) để log

### Tracing
X-ray cho phép người dùng xem được flow of requests (luồng yêu cầu) được gửi qua ứng dụng 

Người dùng cũng có thể có phép 'Active Tracing' trong một function

```
# enable active tracing 
aws lambda update-function-configuration --function-name my-function --tracing-config Mode=Active
```

**AWSXRayDaemonWriteAccess** là policy có thể dùng cho role 

### Layers
Nếu trong trường hợp có hai lambda functions dùng cùng libraries với nhau, thì người dùng có thể chọn tạo ra một layer riêng để install libraries để các functions có thể dùng chung. Layer để install được để vào **/opt** folder. 
- Dùng layers có thể tiết kiệm thời gian mỗi khi chạy nhiều function bởi Function code sẽ nhỏ hơn và chạy nhanh hơn


### Container Images - Dùng CI/CD
Để có thể tự test lambda functions tại máy local trước khi deploy (triển khai) thì người dùng có thể để vào Container Image - Lambda Runtime API và dùng AWS Lambda Runtime Interface Emulator (RIE) để test

Và sau khi test xong lambda function tại local, thì container mới sẽ gỡ layer RIE và tạo một container image để gửi tới Amazon Elastic Container Registry (ECR). 

## Lambda và ALB

Khi yêu cầu của người dụng được nhận ở ALB qua HTTP/HTTPS, Lambda sẽ được invoke sẵn (synchronously) và sẽ ở trong trạng thái đợi (waits for response). Sau đó ALB sẽ gửi Event tới Lambda dưới dạng JSON. 

ALB ở giữa sẽ dịch yêu cầu của người dùng từ HTTP/HTTPS tới JSON và từ JSON tới HTTP/HTTPS để gửi tới người dùng 

### Multi-value Headers
Multi-value Headers là tính năng cho phép ALB gửi một array với nhiều yêu cầu từ người dùng tới lambda. 

Nếu không dùng multi-value headers thì lambda chỉ nhận được yêu cầu cuối cùng từ người dùng cho dù người dùng gửi nhiều yêu cầu khác nhau.

## Resource policy
Mỗi lambda function có hai tầng bảo mật:
1. Execution Role - được assume bởi function và quyết định lambda function được làm gì và cho phép lambda function dùng các resources của AWS 
2. Resource policy - dùng để cho phép AI được LÀM Gì với lambda function

Nếu trong cùng một account:
- Thì chỉ cần một resource policy hoặc identity policy để cho phép user sử dụng lambda function 

Nếu muốn một user tại account A dùng function tại account B, tuy nhiên account B không trust account A thì: 
- Phải dùng resource policy tại lambda function tại account B để cho phép user tại account A sử dụng

Nên dùng resource policy cho"
- Dịch vụ AWS cần invoke lambda function 
- cho phép cross-account dùng lambda function