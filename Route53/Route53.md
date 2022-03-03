## Hosted Zone
- Một R53 Hosted Zone là một cở sở dữ liệu DNS cho tên miền
- Hosted Zone được tạo theo mặc định khi đăng ký tên miền trên R53
- Phí dùng hosted zone được tính theo:
    + Phí hosting hàng tháng cho mỗi hosted zone 
    + Phí cho mỗi query tới hosted zone
- Hosted zone được dùng để host các dữ liệu DNS (vd. A, AAAA, MX, TXT)
 
### Public Hosted Zones
- Cơ sở dữ liệu DNS (zone file) hosted bởi R53 (Public Name Servers)
- Có thể cập nhật qua Internet và VPCs
- Zonefile sẽ được host trên **4** R53 Name Server của vùng đó
- Dữ liệu được lưu được gọi là Resource Records 

### Private Hosted Zone 
- Private Hosted Zone được kết hợp với VPCs và chỉ được truy cập từ VPCs đó

### Split View Hosted Zone 
- khi Hosted Zone có thể được truy cập bởi private và public trong cùng domain nhưng records khác nhau. 

### CNAME vs ALIAS
CNAME được dùng để kết nối hai NAME với nhau.

VD:
    www.catagram.io => catagram.io

CNAME không được dùng cho tên miền không (naked/apex) - vd. catagram.io

ALIAS được dùng để nối NAME với AWS resource (tài nguyên)

Và có thể được dùng với naked/apex và record bình thường 

## Simple Routing
Dùng Simple Routing khi muốn route tới 1 dich vụ vs. web server

=> Không có health check - nghĩa là không check được nếu record có dùng được hay không

## Health Check
Health Check được dùng để theo dõi hiệu suất của ứng dụng web, web server, và các resources khác. 

- Health check được dùng vởi records 
- Health checker được nằm trên toàn cầu 
- Health checker check mỗi 30s (hoặc 10s nếu thêm phí)
- Nếu trên 18% các health checker báo cáo vận hành tốt thì health check được tính là tốt

## Failover Routing
Dùng failover routing khi muốn cấu hình thêm web dự phòng cho trường hợp primary bị fail

## Multi Value Routing
Multivalue routing cho phép người dùng cấu hình R53 để trả về nhiều giá trị, như IP addresses cho mỗi DNS query.
=> Multi Value cải thiện tính khả dụng (availability) nhưng không phải để thay thế load balancing 

## Weighted Routing 
Cho phép người dùng kết hợp nhiều resources với một tên miền và chọn bao nhiêu traffic được đưa tới mỗi resource. 
=> Dành cho load balancing đơn giản hoặc để test version phần mềm mới 

## Latency Routing 
Khi ứng dụng của bạn được hosted ở nhiều khu vực AWS, có thể cải thiện năng suất cho người dùng bằng cách serve yêu cầu từ AWS Region có độ trễ thấp nhất. 

=> Dùng cho better performance + user experience

## Geolocation Routing
Cho phép người dùng chọn resources serve traffic dựa trên vị trí địa lý của người dùng, nghĩa là từ vị trí của DNS queries. 

=> Dùng khi muốn hạn chế nội dung tuỳ theo vị trí địa lý (vd. nội dung chỉ dành cho người dùng ở Mỹ)

## Geoproximity Routing 
Cho phép R53 route traffic tới resources dựa trên vị trí địa lý của người dùng và resources.

Có thể dùng bias => "+" hoặc "-" để bạn chọn cho người dùng được route tới một resource nhất định


