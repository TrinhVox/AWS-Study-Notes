## Elastic Load Balancer
Load Balancer được dùng để chấp nhận yêu cầu kết nối từ người dùng và chuyển kết nối tới ứng dụng ở phần backend. 
Có 3 loại load balancers trong AWS:
1. Classic Load Balancer - v1
    - không phải layer 7, và thiếu nhiều tính năng, chỉ có 1 SSL mỗi CLB
2. Application Load Balancer - v2
    - Hỗ trợ HTTP/S/WebSocket
3. Network Load Balancer - v2
    - Hỗ trợ TCP,TLS và UDP

### Elastic Load Balancer Architecture
Khi cấu hình ELB, người thiết kế cần quyết định một vài thứ:
1. Chỉ dùng IPv4 hoặc là dual stack (IPv4 + IPv6)
2. Chọn AZ và subnet mà Load Balancer sẽ dùng - Sau khi chọn subnet ELB sẽ để nodes vào mỗi subnet trong AZ. 
    - Chỉ được chọn một subnet trong mỗi AZ 
    - Mỗi ELB sẽ được cấu hình với 1 **A record** DNS để dẫn đến các nodes. 
3. Chọn nếu muốn ELB kết nối với Internet hoặc chỉ dùng trong mạng private
    - nếu chọn kết nối với Internet thì mỗi node của ELB sẽ được tạo một public IP
    - nếu chọn kết nối internal thì mỗi node sẽ được tạo một private IP

=> Internet facing LB nodes có thể truy cập public VÀ private EC2 Instances
=> Internal LB thươngd được dùng để scale giữa application tiers

Để LBs hoạt động cần có 8+ IPs trống trong mỗi subnet và subnet được chia /27 hoặc lớn hơn để có thể scale 

#### Cross-zone LB

Khi yêu cầu được gửi từ người dùng, thường load sẽ được chia đều cho các nodes. Node trong mỗi AZ sẽ kết nối được với các Instance trong cùng AZ nên load sẽ được chia đều cho các instances trong AZ. 

Cross-zone LB cho phép Node trong một AZ chia đều load cho tất cả Instance trong các AZs. 

#### Application Load Balancer
- Layer 7 Load Balancer - nghe HTTP và HTTPS
- Có thể nghe nội dung của Layer 7 vd: cookies, customer header, user location, app behaviour
- Kết nối HTTP và HTTPs được ngưng tại ALB và kết nối mỡi sẽ được tạo ra với ứng dụng
- ALBs cần phải có SSL nếu HTTPs được dùng 
- ALBs chậm hơn so với NLB và có nhiều levels trong network stack để xử lý 
- Lợi ích khi dùng ALB là có health checks để kiểm tra sức khoẻ của ứng dụng (layer 7)
- ALBs sẽ xử lý kết nối dùng rules và rules được xử lý theo priority order (thứ tự ưu tiên)
- ví dụ Rules Condition: host-header, http-header, http-request-method, path-pattern,...
#### Network Load Balancer
- Layer 4 Load Balancer - dùng TCP, TLS, UDP, TCP_UDP
- Không thấy hoặc hiểu được HTTPs và HTTPs
- Không hiểu nội dung của layer 7 
- Rất nhanh
- Nên được dùng với SSH, SMTP, Game Servers hoặc ứng dụng financia;s 
- NLBs có thể dùng để whitelisting
- Có thể chuyển tiếp kết nối thẳng tử TCP đến Instances

Nên dùng NLB khi:
+ Không muốn ngưng mã hoá dữ liệu từ người dùng đến Instances
+ Dùng để whitelist static IPs
+ Cần độ trễ ít 
+ Cần privatelink

### SSL Offload 
3 cách để ELB xử lý SSL:
1. Bridging (mặc định)
Listener được cấu hình cho HTTPs. Kết nối từ người dùng đến ELB sẽ ngưng(terminated) và cần giấy chừng nhận cho tên miền (domain name) của Application. Nghĩa là ELB sẽ thấy được content của HTTPs và action theo content đó. Instances cần SSL Certificate và compute cần để mã hoá. 
2. Pass-through
NLB chỉ forward (chuyển tiếp) kết nối từ client đến application. Listener được cấu hình cho TCP. Instances cần SSL Certificate và compute cần để mã hoá, AWS sẽ không có dữ liệu gì về Certificate và tất cả được quản lý bởi người dùng. 
3. Offload
Listener được cấu hình cho HTTPs. Kết nối từ người dùng tới ELB sẽ ngưng và tiếp tục kết nối tới application bằng HTTP.

### Session Stickiness

Connection Stickiness là tính năng ELB để giữ cache dữ liệu của người dùng. Connection Stickiness dùng cookies (AWSALB) để giữ device với một instance trong backend cho một khoảng thời gian nhất định (1 giấy tới 7 ngày) cho tới khi:
1. Máy instance ở backend bị fail 
2. Có tới khi cookies hết hạn

