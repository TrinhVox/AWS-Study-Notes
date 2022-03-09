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

### Errors
- 4XX- Client errors - Request từ client có lỗi 
- 5XX - Server errors - Request đúng, nhưng có lỗi trong backend
- 400 - Bad Request - Generic
- 403 - Access Denied - WAF Filtered
- 429 - API Gateway can throttle - nghĩa là client đã dùng hết dung lượng 
- 502 - Bad Gateway Exception - đầu ra từ lambda bị lỗi 
- 503 - Service Unavailable - backend không trong hoạt động
- 504 - Integration Failure/Timeout - giới hạn trong **29s**

### Caching
Cache có thể có kích thước từ 500MB tới 237 GB

Mặc định Cache là 300s (min 0 - max 3600s)


