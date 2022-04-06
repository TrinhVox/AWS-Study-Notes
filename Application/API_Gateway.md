## API Gateway

API Gateway là dịch vụ cho phép tạo API Endpoints, Resources (tài nguyên) và Methods. 

Nằm giữa ứng dụng và dịch vụ backend

Có thể có HTTP APIs, REST APIs và WebSocket APIs. 

Có hai phần trong API Gateway:
1. Request - bao gồm:
    - Authorize
    - Validate
    - Transform
2. Response - bao gồm:
    - Transform
    - Prepare
    - Return

### Authentication 

API Gateway có thể dùng Cognito để authenticate và gửi token. 

API Gateway cũng có thể dùng Custom Authorisation bằng cách lấy token từ client và gọi Lambda Authorizer để check token ID. 

### Endpoint Types
- Edge-OPtimized - dùng Cloudfront 
- Regional - khi clients từ một khu vực 
- Private - endpoint chỉ được truy cập trong VPC

### Stages
API được triển khai theo giai đoạn, mỗi giai đoạn có một deployment
Vd: prod, dev, test

### Errors
- 4XX- Client errors - Request từ client có lỗi 
- 5XX - Server errors - Request đúng, nhưng có lỗi trong backend
- 400 - Bad Request - Generic
- 403 - Access Denied - WAF Filtered
- 429 - API Gateway can throttle - nghĩa là client đã dùng hết dung lượng 
- 502 - Bad Gateway Exception - đầu ra từ lambda bị lỗi 
- 503 - Service Unavailable - backend không trong hoạt động
- 504 - Integration Failure/Timeout - giới hạn trong **29s**

=> https://docs.aws.amazon.com/AmazonS3/latest/API/API_Error.html  - Error codes cho S3

### Caching
Cache có thể có kích thước từ 500MB tới 237 GB

Mặc định Cache là 300s (min 0 - max 3600s)


### Integrations
API Gateway có thể kết nối với Lambda, HTTP Endpoints, Step Functions, SNS, DDB

Có nhiều loại integration khác nhau:
1. MOCK - dùng cho testing và không cần kết nối backend
2. HTTP - dùng cho HTTP Endpoint và cần dung mapping templates để chuyển thông tin cho backend
3. HTTP Proxy - API Gateway sẽ gửi thông tin từ frontend tới backend mà không cần thay đổi thông tin 
4. AWS - cho phép kết nối với dịch vụ của AWS - nhưng phải tự cấu hình cách API Gateway thay đổi thông tin cho AWS services
5. AWS_Proxy (Lambda) - Cho phép gửi thông tin từ frontend tới lambda mà không cần chỉnh thông tin 

#### Mapping Templates
Dùng để cấu hình các parameters cho integration với AWS và HTTP 

### OpenAPI và Swagger
Dùng để format cho REST API - giống như cloudformation nhưng thay vì cho AWS Service thì dùng cho APIs

Có thể để Input và Output parameters và cách Authenticate

### Exponential Backoff
The idea behind exponential backoff is to use progressively longer waits between retries for consecutive error responses.

Exponential backoff là cách để sửa lỗi có error code 4xx, bằng cách khi gửi các requests, thời gian chờ sẽ lâu hơn giữa mỗi error responses. 