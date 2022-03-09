## Step Functions 

Step Functions là dịch vụ để tạo ra serverless workflow dài mà không thể dùng qua Lambda. 

Serverless workflow => START-> STATE->END

Thời hạn maximum là **1 năm**

Có thể bắt đầu bằng API Gateway, IOT Rules, EventBridge, Lambda...

Dùng Amazon States Language - JSON template

IAM Roles được dùng để giao quyền 

### States

- SUCCEED & FAIL
- WAIT - đợi một thời gian hoặc tới một giờ nào đó
- CHOICE - cho phép người dùng chọn
- PARALLEL 
- MAP - nhận một list nào đó (giống như loop)
- TASK - một đơn vị việc - có thể kết hợp với Lamda, Batch, DynamoDB, ECS, SNS

