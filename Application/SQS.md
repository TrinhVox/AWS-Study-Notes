## Simple Queue Service

SQS là dịch vụ public dùng để quản lý queues (hàng) cho các tin nhắn (messages)

1 requst = 1-10 messages tới 256KB tổng cộng

SQS có hai loại:
1. Standard
    - Làm hiệu quả nhất
    - Đảm bảo được xử lý ít nhất một lần (at-least-once)
2. FIFO (First In First Out)
    - Xếp hàng theo thứ tự 
    - Được xử lý duy nhất một lần (exactly-once)
    - 3000 messages per second with batching, or 300 without  

Messages có thể lớn tới 256KB

VisibilityTimeout là thời gian messages được giấu đi (hidden) trước khi xuất hiện lại hoặc khi client xoá trực tiếp

**Dead-letter queues** được dùng để chứ messages không xử lý được và cần được xử lý riêng

SQS được tính phí theo request 

Có 2 cách Polling:
- Short 
- Long (waitTimeSeconds) - chờ tới khi messages gửi tới queue
