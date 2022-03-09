## Simple Notification Service 
- Public AWS Service - cần kết nối tới Public Endpoint của AWS

SNS là dịch vụ điều phối để gửi thông báo/tin nhắn

Mỗi tin nhắn có trọng tải (payload) = **256KB**

SNS Topics bao gồm permissions và configuration 

Publisher là người gửi message tới TOPIC

TOPIC bao gồm subscribers là người được nhận tin nhắn
 
Có nhiều subsciber types: 
- HTTPs
- Emails (JSON)
- SQS
- Lambda

