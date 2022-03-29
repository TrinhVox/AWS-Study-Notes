## CloudWatch 

### Architecture
- Dịch vụ public - có thể kết nối qua Interface Endpoint
- Có thể kết hợp với On-Premises qua Agent/API
- Kết hợp với ứng dụng qua API/Agent 
- Có thể xem data qua Console UI, CLI, API
- Dùng thêm alarms (báo động)

Từ VPC có thể kết nối tới CloudWatch qua:
1. Internet Gateway
2. Interface Endpoint 

- Data point = Thời gian (timestamp), value và unit of measure (đơn vị tính)
- Dimension - cặp tên và giá trị (name/value)
    + Các loại dimensions gồm:
        1. AutoScalingGroupName
        2. ImageId
        3. InstanceId
        4. InstanceType
- Resolution (phân giải) - standard (thường) là 60s => vd. (CPUUtilisation 40% - cho 60s và sẽ được update mỗi 60s) 
    + Resolution High là 1s

### Logs
- Logs được lưu vào Log Stream 
- Log stream được xếp vào Log Group 
- Log Group có thể dùng để tạo Retention, Permission và Encryption 
- Chỉ có thể dùng SSE-S3 để encrypt

### Subscriptions
- Kinesis Data Firehose để gửi tới S3 với thời gian gần thực (near-real time)
- AWS Managed Lambda functions để gửi tới Elasticsearch (real-time)
- Kinesis Data Stream