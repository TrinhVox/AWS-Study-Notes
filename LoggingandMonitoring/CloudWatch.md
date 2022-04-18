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


### Data
- Namespace = container cho metrics (vd: AWS/EC2 & AWS/Lambda) - tất cả resources của AWS tạo sẽ có namespace bắt đầu bằng AWS/
- Datapoint = unit nhỏ nhất của cloudwatch, Thời gian (timestamp), value và unit of measure (đơn vị tính)
- Metric = một set của data point được xếp theo thứ tự thời gian (vd: CPUUtilization, NetworkIn, DiskWriteBytes dành cho EC2)
    + Mỗi metric có một MetricName (CPUUtilization) và một Namespace (AWS/EC2)
- Dimension = một cặp name/
    + Các loại dimensions gồm:
        1. AutoScalingGroupName
        2. ImageId
        3. InstanceId
        4. 
- Resolution (phân giải) - standard (thường) là 60s => vd. (CPUUtilisation 40% - cho 60s và sẽ được update mỗi 60s) 
    + Resolution High là 1s
- Statistic - aggregation dữ liệu qua một thời gian (vd. Min, Max, Sum, Average)
- Percentile

### Alarms 
Dùng để quản lý một metric qua một thời gian và tạo ra một hoặc nhiều actions

- Alarm Resolution
### Logs
#### Ingestion:
- CloudWatch logs - public service
- CloudWatch cho phép AWS, On-premises, IOT hoặc ứng dụng 
- Dùng CWAgent 
- VPC Flow Logs
- CloudTrail
- Elastic Beanstalk, ECS, API GW, Lambda
- Route53 

CloudWatch Log là Region service nên các log sẽ gửi vào cloudwatch tại region đó. 

Log event bao gồm:
- Timestamp
- Raw Message

Log event sẽ được gôm vào log stream, mỗi log stream là từ cùng một logging source. Log group là một nhóm các log stream.
- Log Group có thể dùng để tạo Retention, Permission và Encryption 
- Log Group có thể được dùng với Metric Filter để tạo Metric

Có thể export logging data tới S3, nhưng sẽ không phải real-time 
- Chỉ có thể dùng SSE-S3 để encrypt

#### Subscriptions
- Kinesis Data Firehose để gửi tới S3 với thời gian gần thực (near-real time)
- AWS Managed Lambda functions để gửi tới Elasticsearch (real-time)
- Kinesis Data Stream


