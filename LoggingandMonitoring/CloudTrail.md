## CloudTrail

Cloudtrail là dịch vụ dành cho kiểm toán (audit), giám sát (monitoring) và troubleshoot bằng cách theo dõi activity của người dùng và API. 

Có thể xem được hoạt động của account trong vòng 90 ngày trước. 

Log files được lưu tại S3 bucket 

Có thể tạo tới 5 trails trong một AWS region. Trail được tạo cho tất cả các region sẽ được tính là 1 trail tại từng region. 

### Data events 
Data events là events dữ liệu dùng để cung cấp thông tin chi tiết về các hoạt động bên trong tài nguyên (data plane). Data events thường là những activies với volume cao. 

Data events thường được dùng cho các dịch vụ như S3 và Lambda. 

Data events cần trả phí thêm. 

### Cloudtrail Insights
AWS Cloudtrail Insights events cho phép người dùng xác định những hoạt động bất thường trong AWS accounts của mình như số lượng tài nguyên được cung cấp tăng đột biến, hoặc IAM activities. 

Cloudtrail Insights dùng machine learning models để liên tục giám sát.

Khi hoạt động bất thường được phát hiện, CloudTrail Insights events sẽ được hiện lên bên trong console, và gửi tới CloudWatch Events, tới S3 bucket hoặc là tới Cloudwatch logs. 

### Cloudtrail Log File Integrity Validation
CloudTrail log file integrity validation là một tính năng cho phép người dùng xác định nếu CloudTrail log file có bị thay đổi, xoá hay không từ lúc log file được giao tới S3 bucket. 

Sau khi log file integrity validation được kích hoạt, CloudTrail sẽ gửi **digest files** theo mỗi giờ. Digest file sẽ bao gồm:
- thông tin về các log files
- hash values cho log files
- digital signatures (chữ ký số) cho digest file trước 
- digital signatures cho digest file hiện tại 

Digest file sẽ được gửi vào cùng S3 bucket với log files, nhưng ở một folder khác bên trong bucket. 

Để validate log files người dùng có thể sử dụng CLI hoặc tự tạo một service riêng để validate. Dùng CLI để validate bằng cách:

```
aws cloudtrail validate-logs --trail-arn <trailARN> --start-time <start-time> [--end-time <end-time>] [--s3-bucket <bucket-name>] [--s3-prefix <prefix>] [--verbose]
```
