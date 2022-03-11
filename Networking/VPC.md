

## VPC Sizing and Structure

Khi chọn CIDR cần nghĩ tới:
- VPCs nên có kích thước bao nhiêu
- Có networks nào mình không dùng được không?
- Cần phải tránh networks đã được dùng rồi hoặc được dùng bởi đối tác...
- Cố gắng nhìn xa tới tương lai nhất có thể 
- Cấu trúc của VPC - các bậc trong cấu trúc của phần mềm và AZ

Các hạn chế khi chọn CIDR VPC:
- VPC subnet ít nhất là /28 (16IP), maximum /16 (65456)

Thường khi chọn bao nhiêu dãy CIDR thì nghĩ về bao nhiêu **region** sẽ được dùng cho infrastructure này

Nên giữ:

=> 2+ network trong mỗi region được dùng bằng mỗi tài khoản

Khi quyết định kích thước của VPC:
- Cần bao nhiêu subnets?
- Cần bao nhiêu IPs tổng cộng? Bao nhiêu IPs trong một subnet?

=> VPC Services run from the subnet, not directly from the VPC

Cách an toàn nhất:
1. Chọn bao nhiêu AZ để chạy service từ, thường dùng 3, cộng thêm 1 buffer dư để nếu có thể thêm AZ => 4 AZ
2. Bao nhiêu bậc (tiers) trong phần mềm? (Thường sẽ có cho web, app, db) + 1 buffer dư => 4 tiers
=> 16 subnets


## Custom VPCs

Mặc định IPs của VPCs là Private (riêng)

VPC sẽ có 1 Primary Private CIDR Block (khối IP CIDR riêng chính) - ít nhất là /28 (16IP), maximum /16 (65456)

DNS đã được tạo bởi R53 cho VPC

DNS IP => Base IP + 2

**enableDnsHostnames** nếu được bật sẽ cho các dịch vụ public trong VPC có DNS Host Names PublicPublic
**enableDnsSupport** nếu được bật các dịch vụ trong VPC sẽ được dùng DNS IP

## VPC Subnets
=> AZ resilient 

1 subnet chỉ ở trong 1 AZ, nhưng 1 AZ có thể có nhiều subnets

Subnets trong một VPC có thể trao đổi với nhau 

Trong mỗi Subnet, sẽ có 5 reserved IP address (IP Address được để riêng):
- Network Address (vd: 10.16.16.0)
- VPC Router (để chuyển dữ liệu qua các subnets) => 10.16.16.1
- Reserved DNS - Network + 2 => 10.16.16.2
- Reserved Future use - Network + 3
- Broadcast address (Địa chỉ cuối trong subnet) => 10.16.31.225

## VPC Router
Mỗi VPC đều có một VPC router

VPC Router dùng để chuyển dữ liệu bên trong VPC qua các subnets

VPC Router được chỉ định bởi các route table (bản chỉ hướng), mỗi subnet đều có 1 bản

VPC có một route table chính

Khi chuyển dữ liệu, nếu IP address được trùng khớp với nhiều address thì address nào có prefix cao hơn thì được ưu tiên

## Internet Gateway

=> Region resilient 
1 VPC có thể có 0/1 IGW, 1 IGW = 0/1 VPC 

IGW được dùng để kết nói VPC với Internet hoặc với AWS Public Zone (S3,SQS, SNS)

1. Tạo IGW
2. Kết nối IGW với VPC
3. Tạo một route table riêng
4. Kết nối route table
5. Gắn default route (IP mặc định - 0.0.0.0/0) dẫn tới IGW
6. Tạo IPv4 hoặc IPv6 cho subnets

IGW giữ các mỗi quan hệ giữa IPv4 riêng và IPv4 công cộng. Hệ điều hành không giữ IPv4 address công cộng. 

## Stateful vs Stateless Firewalls

### Stateless Firewalls

Stateless firewalls là khi firewall (tường lửa) không biết trạng thái của kết nối là gì (outbound hay inbound)

Stateless firewalls cần có hai qui định: 1 vào, 1 ra

Do stateless firewalls không biết được trạng thái của kết nối, nên qui định cần gồm luôn các ephemeral ports (cổng tạm thời) và lý do này nên stateless firewalls không an toàn. 

### Stateful Firewalls

Stateful firewalls có thể biết được trạng thái của kết nối, cái nào là yêu cầu và cái nào là câu trả lời

Khi đồng ý cho request (yêu cầu) thì response (câu trả lời) sẽ được tự động cho phép

Chỉ cần biết được địa chỉ IP và port (cổng), không cần biết là hướng vào hay ra. 

## Network Access Control Lits (NACL)

NACL là tường lửa truyền thống được hoạt động trong VPCs. 

Mỗi subnet có một NACL, để lọc kết nối đi ra vào subnet.

Nhưng các instances trong một subnet sẽ không bị vướng qui định của NACL

Qui định trong NACLs sẽ xét nếu trùng IP, cổng của điểm đến và xem nếu kết nối được cho phép hay không. 

Qui định được xử lý theo thứ tự, từ qui định có **số nhỏ nhất trước**. Khi đúng IP và cổng thì qui trình xử lý dừng. Nếu không có qui định cho IP thì sẽ phủ nhận (implicit deny)

NACLS là Stateless Firewalls

NACLs sẽ được áp dụng với các kết nối trong VPC, tới VPC và từ VPC

Mỗi subnet chỉ có 1 NACL (default hoặc custom), nhưng 1 NACL có thể được áp dụng cho nhiều subnets

=> NACL dùng để chặn kết nối không muốn tới VPC

## VPC Security Groups (SG)

SG là tường lửa stateful. 

Nhưng không được explicitly deny - nghĩa là không được từ chối ai riêng biệt được hết

Nên thường sẽ được dùng chung với NACLs

SG được gắn với ENI chứ không phải subnet

SG reference có thể dùng để kết nối với bất cứ thứ gì có đính kèm SG. Logical reference có thể scale rất tốt, nghĩa là nếu có thêm tài nguyên được cộng vào SG thì các reference được tạo từ trước đều có thể áp dụng với tài nguyên mới. 

=> SG dùng để cho phép kết nối với VPC


## Network Address Translation (NAT)

NAT là một qui trình dùng để thay đổi địa chỉ IP điểm đến hoặc điểm bắt đầu của một packet.

IGW dùng static NAT để đổi địa chỉ IP riêng thành địa chỉ IP công cộng để kết nối với internet

NAT dùng để IP masqerading (dùng một IP public cho một dãy CIDR IP private)

NAT được dùng để kết nối Private CIDR tới Internet, chứ không thể kết nối Internet tới CIDR

NAT Gateway sẽ giữ translation table (bảng dịch địa chỉ) bao gồm source address (địa chỉ điểm gửi), destination address (địa chỉ điểm đến), cổng gửi, cổng đến. 

Để kết nối private subnet tới internet, private subnet có thể kết nối với NAT Gateway ở public subnet để kết nối với internet. 

NAT Gateways:
 - được đặt ở public subnet 
 - dùng Elastic IPs
 - AZ resilient

NAT Gateway tính phí bao gồm:
- phí cơ bản NAT Gateway tiêu thụ trong khi chạy 
- phí dựa trên lượng dữ liệu được xử lý

Để thiết kế một VPC với đầy đủ khả năng phục hồi thì, mỗi AZ cần có một NAT Gateway, để tất cả subnet trong AZ đó có thể kết nối với. Như vậy, khi một AZ có lỗi thì vẫn có các AZ khác hoạt động.                         

NAT Gateway không thể áp dụng SG, chỉ có thể dùng NACL với NAT Gateway

## VPC Flow Logs
Chỉ có metadata, không có content

Có thể được đings kèm với VPC - tất cả ENIs trong VPC
    - Hoặc tất cả ENIs trong subnet
    - ENI trực tiếp

Flow logs không phải thòi gian thực 

Log được gửi tới S3 hoặc cloudwatch logs hoặc Athena

## VPC Endpoints

### Gateway Endpoints
- Cho phép truy cập private tới S3 và DynamoDB
- Thêm Prefix List vào route table để dẫn tới Gateway Endpoint
- HA trên toàn AZ
- Endpoint policy dùng để quản lý cái gì được truy cập 
- Chỉ có thể truy cập dịch vụ trong cùng khu vực

### Interface Endpoints
- Cho phép truy cập private tới AWS Public Services
- Không phải HA mà chỉ được đặt vào subnet 
- Có thể dùng Security Groups để quản lý access
- Dùng Endpoint policies để quản lý
- Chỉ được dùng với TCP và IPv4
- Dùng PrivateLink
- Dùng DNS mới cho endpoint

## VPC Peering
VPC Peering là dịch vụ để kết nối hai VPCs với nhau bằng encrypted network link
- Có thể dùng cho cùng/khác khu vực hoặc cùng/khác tài khoản
- Cần Route table để kết nối và cần cấu hình SGs và NACLs để cho phép kết nối
