## EC2 Architecture

EC2 Instances là các máy tính ảo (gồm hệ điều hành + tài nguyên)

EC2 Instances được chạy trên các EC2 Hosts (máy chủ EC2). Các hosts (máy chủ) có thể dùng chung (shared hosts) hoặc dùng riêng (dedicated hosts)

=> EC2 là AZ resilient

Host được đặt ở AZ, và Host gồm có Instance Store, storage (bộ nhớ lưu trữ dữ liệu), Data Network (mạng lưới dữ liệu)

Mỗi instance được tạo ra ở một subnet trong AZ sẽ được tạo một Elastic Network Interface (ENI) để kết nối với host. Instances có thể có nhiều ENI ở nhiều subnets khác nhau, chỉ cần các subnet đó ở chung một AZ.

EC2 có thể dùng bộ nhớ lưu trữ từ xa nếu kết hợp với EBS (Elastic Block Storage). EBS chạy riêng cho mỗi AZ.

EC2 sẽ đổi host trong hai trường hợp:
1. Host được AWS tắt để sửa chửa 
2. EC2 bị tắt và bật lại (không phải restart)

Khi nào dùng EC2?
- Khi cần dùng OS và phần mềm theo cách truyền thống 
- Khi cần dùng trong thời gian dài
- Burst or steady-state load
- Server style applications
- Monolithic application stacks
- Disaster recovery

## EC2 Instance Types

Khi quyết định chọn loại EC2 nào cần nghĩ tới:
- CPU thô, bộ nhớ, dung lượng và loại lưu trữ
- Resource ratios - tỷ lệ tài nguyên 
- storage and data network bandwidth - lưu trữ và băng thông mạng dữ liệu
- System architecture - kiến trúc hệ thống

Các thể loại EC2:
- General Purpose (mục đích chung) - loại mặc định - với khối lượng công việc đa dạng, và tỷ lệ tài nguyên bằng nhau
- Compute Optimized (điện toán tối ưu hoá) - thường được dùng cho xử lý hình ảnh, video, machine learning, gaming - nhiều CPU 
- Memory Optimized (bộ nhớ tối ưu hoá) - thường được dùng khi cần xử lý khối lượng lớn dữ liệu trong bộ nhớ, và khối lượng công việc dành cho cơ sở dữ liệu
- Accelerated Computing (điện toán nhanh) - khi cần GPU riêng và cần các tính năng khác - field programmable gate arrays
- Storage Optimized (lưu trữ tối ưu)

Ví dụ Instance Type: R5dn.8xlarge
- R: Instance family
- 5: Instance Generation
- 8xlarge: Instance Size
- dn: chức năng bổ sung

## EC2 Purchase Options

### On-demand (mặc định)
Máy ảo on-demand được chạy trên cùng phần cứng nhưng mỗi máy đều được cô lập. 

Máy ảo với diện tích khác nhau đều chạy trên cùng một mấy chủ EC2. 

Máy ảo on-demand được tính phí theo giây máy được chạy. Nhưng bộ lưu trữ vẫn được chạy dù máy tắt nên vẫn được tính phí. 

On-demand không bị ngắt, không được đặt trước những dung tích còn dư, phí sử dụng được tính theo giá cố định và không có giảm giá. 

=> On-demand thường được dùng cho công việc ngắn hạn
 
### Spot

Khi máy chủ EC2 còn dư dung lượng cho máy ảo, AWS sẽ bán dung lượng cho máy ảo với giá được giảm tới 90%. Giá được định tuỳ vào dung lượng dư vào thời điểm đó. 

Khi người dùng sử dụng Spot Price, người dùng phải cho giá tối đa có thể trả cho dung lượng người dùng muốn. Nếu giá spot price AWS đưa ra được tăng trên giá tối đa của người dùng, máy ảo sẽ bị tắt (terminated)

=> Không nên dùng Spot cho công việc không thể bị ngắt hoặc có gián đoạn 

### Reserved

Máy ảo Reserved được dùng cho công việc dài hạn, khi cần được dùng máy ảo liên tục trong thời gian dài.

Khi máy ảo được để dành thì phí theo giây sẽ giảm hoặc không tính.Nghĩa là khi người dùng đăng ký đặt trước máy ảo, người dùng phải trả phí cho máy dù không dùng đến. Nhưng tài nguyên sử dụng trong dung lượng đã được đặt trước sẽ rẻ hơn. 

Máy ảo có thể được đặt trước cho 1 năm hoặc 3 năm và sẽ phải trả trước cho toàn thời gian được đặt. 

Có 3 cách để trả phí:
- Không trả trước, trả giá theo phí trên giây
- Trả tất cả trước => được giảm giá nhiều nhất
- Trả trước một nửa và trả một nữa theo phí trên từng giây

#### Scheduled Reserved 
Máy ảo sẽ được đặt trước và chỉ được dùng theo lịch đã đặt trước.

#### Capacity Reservations
Khi người dùng cần dùng máy ảo nhưng không muốn có gián đoạn, người dùng có thể đặt trước dung lượng theo AZ và phải trả phí dù không sử dụng. 

### Dedicated Host
Máy chủ chuyên dụng, nghĩa là máy chủ chỉ được dành riêng cho người dùng.

Người dùng sẽ trả phí cho máy chủ và không cần trả tiền cho máy ảo được tạo trên máy chủ đó. Người dùng có thể tạo bao nhiêu máy ảo cũng được trên dung lượng của máy chủ đã mua. 

### Dedicated Instances

Máy ảo chuyên dụng, nghĩa là người dùng không phải chia sẻ máy chủ với người khác. Người dùng sẽ phải trả phí cao hơn cho mỗi máy ảo nhưng không cần phải quản lý phần cứng của máy chủ. 


## Status Check 

Kiểm tra trạng thái của máy ảo gồm 2 phần:
1. System reachability check - Kiểm tra khả năng tiếp cận hệ thống 
    
    - Xem nếu hệ thống mất điện
    - Kiểm tra kết nối mạng 
    - Kiểm tra phần mềm và phần cứng của máy chủ 

2. Instance Reachabillity check - kiểm tra khả năng tiếp cận máy ảo

    - Kiểm tra hệ thống tệp tin (file system)
    - Kết nội mạng trên máy ảo bị sai
    - Lỗi hệ điều hành 

## Instance Metadata

Instance Metadata là dịch vụ được EC2 cung cấp cho người dùng về thông tin của máy ảo. 

**http://169.254.169.254/latest/meta-data**

Instance Metadata là dữ liệu về máy ảo mà người dùng có thể sử dụng để cấu hình hoặc quản lý máy ảo đang sử dụng. 

## Bootstrapping

Bootstrapping là khi người dùng có thể cấu hình sẵn để chạy script khi khởi động máy. Bootstrapping cho phép EC2 tự động hoá. 

Bootstrapping được lắp đặt qua user dât - được truy cập qua meta-data IP

**http://169.254.169.254/latest/user-data**

Bootstrapping được chạy bởi hệ điều hành của máy ảo và chỉ được chạy khi máy khởi động lần đầu tiên.

AMI sẽ được dùng để tạo máy ảo và EC2 sẽ gửi User Data nếu người dùng có cấu hình trước User-data. Máy ảo sẽ dùng User-Data để cấu hình thêm cho máy. 


### User-data

User-data không được đảm bảo an toàn, nên không nên để passwords. 

User-data có giới hạn tới **16 KB**

User-data có thể được sửa lúc máy ngưng hoạt động. 

### Boot-Time-to-Service-Time

Là thời gian từ lúc máy khởi động tới lúc máy sẵn sàng để dùng. 


## EC2 Instance Role

Máy ảo có thể dùng IAM Role để có quyền truy cập vào các tài nguyên khác của AWS. 

Để máy ảo có thể dùng tên đăng nhập và mật khẩu của IAM Role, máy ảo cần có một profile gọi là InstanceProfile. Profile sẽ được đính kèm với máy và credentials sẽ được cập nhật qua meta-data:
**iam/securty-credentials/role-name**

## Parameter Store 

 Parameter store là dịch vụ theo khu vực

 SSM Parameter Store là dịch vụ nằm trong System Manager để lưu trữ các cấu hình và bí mật một cách bảo mật nhất. 

 Tất cả dịch vụ của AWS (Lambda, EC2,..) đều có thể yêu cầu quyền truy cập qua Parameter Store. 

 Có 2 tier trong parameter:
 1. Standard - miễn phí
 2. Advanced

 Code để kết nối với Parameter store từ CLI:
 ```
 aws ssm get-parameter --names /path-to-parameters/parameters

 aws ssm get-parameters-by-path --path /path-to-parameters/

 # to decrypt an encrypted secure string
 aws ssm get-parameters-by-path --path /path-to-parameters/ --with-decryption
 ```

## Logging EC2

Dùng CloudWatch để xem các metrics và log của EC2. 

Để cho phép CloudWatch quyền xem được dữ liệu trong máy ảo thì cần có CloudWatch Agent. Để cloudwatch agent có thể hoạt động thì cần được cấu hình và cấp quyền truy cập.

Cầu hình gồm có các metrics và phần người dùng muốn được log và sẽ được đưa cho CloudWatch Agent. 

Và các Log sẽ được đưa cho CloudWatch Logs, nhưng để CLoudWatch Logs có thể truy cập thông tin, CloudWatch Logs cần có quyền qua IAM Role tới máy ảo.

Các quyền trong IAM Roles Agent cần có là:
1. CloudWatchAgentServerPolicy
2. AmazonSSMFullAccess

Code để tải CloudWatch Agent về máy ảo:
```
# download
wget https://s3.amazonaws.com/amazoncloudwatch-agent/amazon-linux/amd64/latest/amazon-cloudwatch-agent.rpm

# install
sudo rpm -U ./amazon-cloudwatch-agent.rpm

# start Agent
sudo /apt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard

# install a package that the Agent need
sudo mkdir -p /usr/share/collectd/
sudo touch /usr/share/collectd/types.db

# start up the Agent with the configured parameters
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c ssm:AmazonCloudWatch-linus
```

## EC2 Placement Groups
Placement groups là các nhóm mà người dùng có thể dùng để quản lý các máy ảo theo nhóm một cách dễ dàng hơn. 

Placement groups gồm có 3 loại nhóm:
1. Cluster Placement Groups (Performance) - năng suất
- Giữ các máy ảo gần với nhau
- Các máy ảo được hoạt động trong cùng AZ và cùng rack (có thể là cùng máy chủ)
- Các máy ảo có kết nối trực tiếp với nhau 
- **10Gbps per stream** vs. **5Gbps normally**
- Bởi được kết nối trực tiếp với nhau nên placement group này có độ trễ thấp nhất
- Nên khởi động tất cả máy ảo cùng lúc

=> Nên dùng nếu cần tốc độ và năng suất nhanh 

2. Spread Placement Groups (Resilience) - khả năng phục hồi
- Giữ các máy tách biệt
- Các máy được hoạt động trong nhiều AZ và nhiều rack khác nhau
- Có giới hạn **7 máy** trong một AZ
- Không được dùng cho Dedicated Instances hoặc Hosts

=> Nên dùng khi có một số lượng nhỏ máy quan trọng cần được để tách biệt với nhau 

3. Partition Placement Groups (Topology Awareness) - nhận thức về cấu trúc liên kết
- Dành cho các ứng dụng được phân phối và nhân rộng (distributed and replicated) mà có nhận thức về cấu trúc (architectual aware).
- Khi cần nhiều hơn 7 máy và muốn giữ các máy tách biệt với nhau
- Máy ảo được để ở nhiều AZ, máy ảo được hoạt động trong partitions (nhóm nhỏ) và có thể có 7 nhóm trong 1 AZ 
- Các nhóm không được dùng chung rack 
- Không có giới hạn bao nhiêu máy ảo trong một partition

