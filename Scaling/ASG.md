## Auto Scaling Groups (ASG)

Auto Scaling Group được dùng để tự động quản lý và scale (nhân rộng) máy ảo - EC2. 

ASG dùng Launch Templates và Launch Configurations để scale máy theo.  

Được scale dựa trên size minimum, desired (mong muốn), maximum - vd. 1:2:4

ASG sẽ giữ instances chạy theo size desired

=> ASG là miễn phí nhưng resources trong ASG vẫn tính phí


### Scaling Policies
ASGs không cần Scaling Policies nhưng có thể có 1 

Scaling Policies được dùng để tự động hoá dựa trên metrics:
- Dùng rules (luật) để chỉ định scales
- Có thể dùng Manual Scaling - người dùng tự thay đổi desired capacity - dung lượng mong muốn
- Scheduled Scaling - thay đổi theo thời gian được đật trước - vd: thời điểm sales
- Dynamic Scaling - thay đổi theo luật được đặt sẵn:
    + Simple - "Khi CPU xài hơn 50% +1"
    + Stepped Scaling - Scale dựa trên difference vd: Nếu CPU giữa 50% tới 60% không làm gì, 70-80% thêm instance...
    + Target Tracking - vd: Khi cần giữ CPU trung bình là 40%
- Scale theo SQS 

### Lifecycle Hooks

Lifecycle Hooks là tính năng cho phép người dùng thực hiện các hành động tuỳ chỉnh (custom actions) bằng cách cho Instance tạm ngưng cho tới khi:
1. Timeout
2. hoặc khi được kích hoạt lại  bằng CompleteLifecycleAction

Dùng EventBridge hoặc SNS 

### Health Checks
ASG health check bằng cách:
- EC2 (mặc định)
    + Unhealthy khi - Không phải Running 
- ELB (Có thể được cho phép)
    + Healthy khi - Running và thông qua ELB health check
- Custom (tuỳ chỉnh)
    + Thêm system để check

Health check grace period là thời gian chờ trước khi check - mặc định là **300s** 