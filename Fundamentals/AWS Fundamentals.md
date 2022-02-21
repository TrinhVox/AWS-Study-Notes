# AWS Fundamentals
## Public vs Private Services
### "AWS Private" Zone
    Virtual Private Clouds (VPC) được biệt lập trừ khi được cấu hình khác. VPCs không kết nối được với nhau và Internet trừ khi người dùng cho phép. Các dịch vụ ở trong private zone có thể kết nối với Internet hoặc AWS Public Zone qua IGW (Internet GateWay).
### "AWS Public" Zone
    AWS Public Zone là một network được kết nối với mạng Internet. Dịch vụ công cộng của AWS được vận hành ở AWS Public Zone, ví dụ như dịch vụ lưu trữ dữ liệu (S3) có public endpoint thì sẽ được vận hành tại zone này. 

## Global Infrastructure
### AWS Regions - Khu vực
Mỗi khu vực đều có đầy đủ các dịch vụ và có nhiều Availability Zone (Vùng Sẵn Sàng). Mỗi khu vực đều được tách biệt hoàn toàn để cô lập mọi sự cố và đảm bảo độ sẵn sàng. Người dùng có thể kiểm soát địa điểm vận hành dịch vụ nên có thể đảm bảo hiệu năng. 

Mỗi khu vực sẽ có tên và mã vùng. Vd: Asia Pacific (Sydney) có mã là ap-southeast-2

Mỗi vùng sẵn sàng đều có trung tâm dữ liệu, nguyền điện và mạng kết nối riêng biệt

- Khả năng phục hồi toàn cầu - global resilient:

    Dịch vụ được vận hành toàn cầu và dữ liệu được nhân bản ra nhiều khu vực khác nhau để đảm bảo độ sẵn sàng của dịch vụ và khả năng chịu lỗi cao (fault tolerance). Ví dụ như IAM và Route53.

- Khả năng phục hồi khu vực - region resilient:

    Dịch vụ được vận hành ở một khu vực và có dữ liệu riêng ở khu vực đó, riêng biệt với các khu vực khác. Dữ liệu ở trong khu vực sẽ được nhân bản ra ở mỗi vùng sẵn sàng. Nên nếu có lỗi ở một vùng thì dịch vụ vẫn hoạt động, nhưng nếu lỗi ở cả khu vực thì dịch vụ không thể hoạt động. 

### AWS Edge Locations
Điểm phân phối địa phương - Sẽ chia nhỏ hơn so với khu vực và phủ rộng hơn. Edge Location sẽ phù hợp cho những khách hàng cần độ trễ thấp hơn ví dụ như Netflix vì dữ liệu sẽ ở gần người dùng hơn. 

## VPCs - Virtual Private Cloud 
VPCs là dịch vụ để tạo mạng cá nhân ở trong AWS mà các dịch vụ cá nhân khác có thể hoạt động ở bên trong. 

VPC là dịch vụ trong khu vực, nên mỗi VPC được tạo ra ở trong 1 tài khoản và 1 khu vực. 

Theo mặc định VPC được tạo cho cá nhân và được tách biệt trừ khi được cấu hình khác. 

Có 2 loại VPCs:
- Default VPC - VPC mặc định:

    Mỗi khu vực chỉ được có 1 VPC mặc định. 

    VPC mặc định được cấu hình sẵn bởi AWS nên sẽ ít linh hoạt hơn VPC đặc chế. 

    Default VPC chỉ có 1 dãy CIDR duy nhất

    Khi tạo VPC, các dịch vụ đính kèm là Internet Gateway (IGW), Security Group (SG) & NACL

    Subnets ở trong default VPCs sẽ được cho sẵn địa chỉ IPv4 công cộng

- Custom VPC - VPC đặc chế:

    Mỗi khu vực có thể có nhiều Custom VPCs.

    Custom VPCs có thể có nhiều dãy CIDR 

Mỗi VPC sẽ được ấn định một dãy địa chỉ IP được gọi là **VPC CIDR**. Mỗi dịch vụ ở trong VPC phải dùng đúng địa chỉ được ấn định của VPC CIDR đó. 

VPCs có khả năng region resilient (phục hồi khu vực) bởi vì mỗi VPC sẽ được chia subnet ở mỗi vùng sẵn sàng khác nhau ở trong khu vực.

## EC2 - Elastic Compute Cloud
EC2 dịch vụ điện toán đám mây dùng để kết nối với máy tính ảo. Mỗi máy tính ảo được người dùng sử dụng được gọi là 1 **Instance**.

EC2 là IaaS (Cơ sở hạ tầng dưới dạng dịch vụ)

EC2 là một dịch vụ cá nhân theo mặc định và sử dụng mạng VPC

EC2 một khi được vận hành sẽ vận hành dưới 1 subnet, và một subnet sẽ được vận hành trong 1 vùng sẵn sàng, nên EC2 chỉ có khả năng AZ resilient (phục hồi vùng).

Mỗi Instance, người dùng có thể chọn kích cỡ và khả năng khác nhau tuỳ vào nhu cầu. 

Có hai loại lưu trữ dữ liệu EC2 có thể chọn là lưu trữ trên host hoặc EBS.

### Billing
EC2 sẽ được tính tiền theo nhu cầu (on-demand) trên từng giây hoặc từng giờ tuỳ vào phần mềm được sử dụng trên Instance đó.

Người dùng sẽ bị tính tiền tuỳ vào CPU và dữ liệu mà Instance sử dụng. 

Nếu người dùng sử dụng EBS thì sẽ bị tính tiền dù không lưu trữ hết dung lượng đã chọn.


### State
- Running 
- Stopped
- Terminated

    Khi terminated, Instance sẽ bị xoá đi hoàn toàn bao gồm dữ liệu trong EBS nếu có. 

### Kết nối tới EC2
Có thể dùng Remote Desktop Protocol (port:3389) để kết nối Window

Để kết nối với Linux Instances thì sẽ dùng SSH protocol (port: 22). Khi tạo SSH key pairs:
- Người dùng giữ Private Key (chìa khoá cá nhân) an toàn
- AWS sữ để Public Key (chìa khoá công cộng) cho EC2 Instance được tạo

=> Chìa khoá cá nhân là cách xác thực 

### AMI - Amazon Machine Image
Dùng để tạo EC2 Instance hoặc có thể được tạo ra bởi EC2 Instance.

Mỗi AMI sẽ bao gồm:
- Permissions (Sự cho phép)
    - Public (Công cộng) - Ai cũng có thể sử dụng AMI để tạo EC2 Instance => vd: Linux và Window AMI của AWS 
    - Owner Implicit (Chủ) - Chỉ người tạo ra được sử dụng
    - Explicit (Ngoài) - Người tạo ra trao quyền cho tài khoản nào đó quyền để sử dụng

- Root volume (Dung lượng)
    - Dung lượng lúc đầu để khởi động hệ điều hành

- Block device mapping  (Bản đồ dung lượng)
    - Để biết được bao nhiêu dùng lượng dùng để khởi động và bao nhiêu dung lượng dùng để lưu dữ liệu

## S3 - Simple Storage Service
S3 có thể được sử dụng toàn cầu, và có khả năng region resilient (phục hồi theo khu vực). Lý do là dữ liệu sẽ được nhân bản lên cho từng vùng an toàn (AZ) ở trong khu vực. 

S3 là dịch vụ công cộng, nên có thể được truy cập khi có mạng Internet, và có thể có nhiều người dùng. S3 không có giới hạn dữ liệu.

Có hai loại lưu trữ trong S3:
1. Objects

    -Có thể xem như tệp tin
    -Có Key, giống như tên của file. Key dùng để xác định Bucket
    -Khi có key, và biết được bucket thì có thể truy cập được object
    -Value của một object là dữ liệu của nó => hình ảnh, video...
    -Mỗi object có thể dùng từ 0 bytes tới 5 TB
    - Mỗi object còn bao gồm: version ID, Metadata, Access Control, Subresources

2. Buckets

    - Mỗi bucket được tạo ra trong một khu vực 
    - Tên của bucket phải là độc nhất trên toàn cầu
    - Têm bucket phải có 3-63 chữ thường, không gạch dưới, phải bắt đầu bằng chữ hoặc số. 
    - Bucket không có giới hạn chứa bao nhiều object
    - Flat Structure => Tất cả object chứa trong bucket đều ngang bằng nhau
    - Soft limit (giới hạn mềm) trong 1 tài khoản: 100 buckets
    - Hard limit (giới hạn cứng) ": 1000 buckets

## CloudFormation

CloudFormation là dịch vụ Infrastructure as Code, dùng để tự động hoá, tạo và nâng cấp cơ sở hạ tằng.

Người dùng tạo những bản mẫu (template) dùng YAML, hoặc JSON và CloudFormation sẽ sử dụng để tự động vận hành.

Phần quan trọng nhất trong template của CloudFormation là phần tài nguyên (resources). 

Phần miêu tả (description) là phần người dùng có thể viết tự do. Nếu template có phần AWSTemplateFormatVersion thì description phải ở ngay dưới đó. 

Metadata dùng để quản dao diện trên Console. 

Parameters là phần cần người dùng điền vào nêú cần. 

Conditions giúp đưa ra quyết định nếu điều khoản được thực hiện. 

Outputs là kết qủa được trả về khi template được thục hiện. 


## CloudWatch

CloudWatch là dịch vụ hỗ trợ chính ở AWS, giúp người dùng xem metrics, logs và để quản lý biến cố.

- Cloudwatch thu gom và quản lý dữ liệu về hoạt động của các dịch vụ trong AWS
- Một số metrics sẽ được AWS hỗ trợ theo mặc định
- Nếu muốn theo dõi một metric nào đó mà không được hỗ trợ theo mặc định của AWS, người dùng có thể dùng Cloud Watch Agent để xem thêm những metrics riêng
- CloudWatch Events - dùng để hỗ trợ nếu có sự cố xảy ra vd như EC2 Instance bị xoá thì CloudWatch Events có thể tạo ra một hoạt động khác. Hoặc là CloudWatch Event có thể hoạt động theo giờ được cấu hình trước. 

### Namespace
Namespace có thể xem như là một **container** chứa để xem dữ liệu, và để chia dự liệu thành nhiều mảng khác nhau để dễ kiểm soát. 

### Metric
Metric là một dãy các dữ liệu được sắp xếp theo thời gian. 
Ví dụ: lượng CPU được dùng, lượng Disk đã dùng

## HA, FT, DR
### High Availability - HA

High Availability (tính sẵn sàng cao) nghĩa là hệ thống được thiết kế nhằm mục đích luôn luôn online và nếu do một lý do gì đó không hoạt động được thì sẽ đượck khắc phục ngay lập tức. Thường thì lỗi sẽ được tự động sửa bằng cách thay đổi các tài nguyên được sử dụng. 

Tính sẵn sàng thường được thể hiện qua chỉ số **uptime**. Vd:

        99.9% = 8.77 tiếng một năm **downtime**

Tính năng sẵn sàng cao không có nghĩa là dịch vụ không thể bị lỗi, mà có nghĩ là phải luôn có chuẩn bị cho những tình huống xấu nhất và sửa lỗi một cách nhanh nhất. Và trong lúc lỗi được sửa, người dùng có thể bị gián đoạn. 

HA có thể được đảm bảo bằng cách, thay thế tài nguyên ngay lập tức, một cách tự đông. Hoặc, có thể có một tài nguyên dự bị có thể được thay thế khi cần. 

**Maximising uptime**

### Fault Tolerace - FT

Fault Tolerance (khả năng chịu lỗi) là một đặc tính cho phép hệ thống tiếp tục hoạt động bình thường trong trường hợp một số thành phần của nó bị lỗi. 

Khác với HA, FT phải đảm bảo hệ thống không bị gián đoạn. Một cách để hệ thống không bị gián đoạn là dùng hơn một tài nguyên cần thiết để khi một cái bị lỗi thì vẫn đảm bảo được tài nguyên khác vẫn còn hoạt động. 

**Operating through failure**

### Disaster Recovery - DR

Disaster Recovery (khắc phục hậu quả) là các kế hoạch hoặc các chính sách được định trước đẻ cho phép hệ thống khắc phục hoặc tiếp tục hoạt động sau thiên tai hoặc thảm hoạ do con người tạo ra. 

**Used when HA and FT don't work**

## DNS - Domain Name System

Hệ thống DNS dùng để dịch đường link mà con người đọc được thành địa chỉ IP mà máy tính có thể đọc được. 

DNS Client => máy tính, điện thoại

Resolver => phần mềm trên điện thoại, máy tính hoặc một server có thể kết nối DNS

Zone (vùng) => Một phần của dữ liệu DNS (vd. amazon.com)

Zonefile => Cơ sở dữ liệu cho mỗi zone

Nameserver => chỗ chứa zonefile

Root Hints => config points at the root servers IP and addresses
Root Server => hosts the DNS root zone
Root Zone => points at TLD authoritative servers

DNS Record Types:
- A www => IPv4
- AAAA www => IPv6
- CName www => For a given zone, CName let you create the equivalent of DNS shortcuts => CName only points to name

## Route53
=> Global service - dịch vụ toàn cầu, globally resilient

1. Dịch vụ đăng ký tên miền (domain)
    - Hỏi registries về root zone 
    - Tạo zonefile cho zone đó
    - Tạo 4 nameservers trên toàn cầu để chứa zonefile
    - Dùng nameserver record để đăng ký tên miền trong registry của root zone đó 
2. Máy chủ lưu trữ các zone của tên miền

