Cả Cloudwatchevents và EventBridge đều được dùng để cho điều kiện nếu events xảy ra và actions sau đó.

## CloudWatchEvents 
 Chỉ có một Event bus mặc định cho tài khoản
## EventBridge
 Có thể có nhiều event buses
- Có hai loại rule trong EventBridge:
    + Rate expressions rules - dùng để định nghĩa rules được chạy thường xuyên
    + Cron expressions rules - dùng để định nghĩa rules chạy vào một thời điểm chính xác (ngày, giờ)

Events được kết cấu theo JSON. 