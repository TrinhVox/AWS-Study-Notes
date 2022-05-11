## AWS Config

AWS Config là dịch vụ được dùng để record các thay đổi trong config của resources qua thời gian. 

Dùng để audit thay đổi trong resource và compliance với các standards được đặt ra

AWS config là dịch vụ Regional và có hỗ trợ cho cross-region và account aggregation 

Khi có thay đổi trong config, AWS Config có thể tạo SNS notification và near-realtime event qua EventBridge và lambda 

Tất cả thông tin sẽ được lưu tại region trong một S3 config bucket 

Có thể dùng config rule, các resources sẽ được evaluate với các config rule (AWS managed rule hoặc custom rule dùng lambda). Nếu resources đáp ứng được condition thì có thể gửi tới EventBridge hoặc SNS để remediate. 

### Config Rule

Config rule dùng để đại diện cho cấu hình mong muốn cho tài nguyên và được dùng để đánh giá các thay đổi cấu hình trên tài nguyên đó. 

**Conformance pack** là một tập hợp các Config rules và remediation action (hành động khác phục)

Có hai loại trigger trong Config rule:
- Configuration changes - mỗi khi config thay đổi, config sẽ chạy evaluation
- Periodic - Config chạy evaluation theo thời gian quy định (vd. mỗi 24 giờ)

Để remediate, Config dùng SSM Automation documents để chỉ định các actions cần được thực hiện trên tài nguyên không tuân thủ theo config rule. 