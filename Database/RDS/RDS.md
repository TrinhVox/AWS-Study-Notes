## Fundamentals
### CAP Theorem 
Chỉ có thể có 2 trong Consistency, Availability, Partition Tolerance (resilience)
### ACID and BASE
1. ACID => RDS - hạn chế scaling
- Atomic - tất cả các phần của transaction phải thành công nếu không thì không phần nào thành công
- Consistent - cơ sở dữ liệu chỉ trong trạng thái hợp lệ dù transactions nào được thêm vào thành công  
- Isolated - Dù các transaction có diễn ra cùng một lúc thì phải được diễn ra độc lập và không ảnh hưởng transaction khác
- Durable - Một khi transaction đã được lưu thành công thì sẽ không bị mất đi
- Tập trung vào Consistency

2. BASE => DynamoDB
- Tập trung vào Availability
- Basically Available - Không phải lúc nào cơ sở dữ liệu cũng nhất quán
- Soft state - Cơ sở dữ liệu không bắt buộc sự nhất quán
- Eventually consistent - Nếu chờ đủ thời gian thì có thể sẽ chờ đến được lúc cơ sở dữ liệu nhất quán

## Relational Database Service (RDS)

Cơ sở dữ liệu dưới dạng dịch vụ - Database-as-a-Service

RDS là dịch vụ máy ảo quản lý cơ sỡ dữ liệu (1+cơ sở dữ liệu)

Người dùng sử dụng server được tạo

RDS hỗ trợ các engines như MySQL, MariaDB, PostgreSQL, Oracle, Microsoft SQL Server và Amazon Aurora

### Database Instance
Một Instance có thể chạy cho một hoặc nhiều engines. 

Khi tạo ra một Database Instance, để truy cập Instance thì bạn sẽ dùng CNAME

Instance có nhiều loại khác nhau, ví dụ:
- db.m5 là loại dành cho general use
- db.r5 là loại dành cho memory optimised 
- db.t5 là loại dành cho burst

DB Instance có thể ở 1 hoặc nhiều AZ. 

Mỗi Instance sẽ có kèm theo một storage - EBS trong cùng AZ. 

Instance sẽ được tính phí theo giờ cho instance đó cộng với phí cho storage như cách thường được tính của EBS. 

Security Group sẽ được kết hợp với mỗi Instance 

### RDS MultiAZ
MultiAz là một tính năng của RDS để tạo ra một bản sao dự phòng cho cơ sở dữ liệu RDS chính tại một AZ khác. 

Bản sao được đồng bộ hoá theo Synchronous Replication

RDS chỉ được truy cập qua CNAME 

Khi có Primary và standby thì CNAME được chỉ tới Primary chứ không thể truy cập tới secondary. 

Synchronous Replication là khi dữ liệu được lưu qua bản sao secondary cùng lúc với khi dữ liệu được viết vào local storage

Khi có lỗi tại Primaryy thì CNAME sẽ được chỉ tới secondary database, trong lúc này có thể xảy ra gián đoạn. 

- Tính năng MultiAZ không phải là dịch vụ miễn phí.
- Không thể dùng MultiAZ để scale thêm để đọc dữ liệu
- **60 - 120** giây để dự phòng khi có lỗi
- Chỉ được dùng trong cùng khu vực (Vùng an toàn trong cùng VPC)

### RDS Backups and Restores

#### Recovery Point Objective (RPO):
- Thời gian từ lần cuối backup đến lúc có lỗi xảy ra
- Số lượng tối đa dữ liệu bị mất
- Số RPO càng nhỏ thì giá càng cao
- RPO có thể được giảm bằng cách thường xuyên backups với thời gian giữa mỗi backup ngắn hơn  

#### Recovery Time Objective (RTO):
- Thời gian từ lúc lỗi xảy ra tới lúc lỗi được sửa
- Thời gian này có thể được giảm bằng cách có hardware dự phòng 


1. Automatic Backups
    - Giống với Manual Snapshots nhưng được tự động hoá
    - Ngoài backup dữ liệu, Automatic Backups còn lưu trữ transactions log mỗi 5 phút => RPO = 5 phút
    - Backups sẽ được tự động xoá đi nên người dùng có thể giữ lại dữ liệu trong vòng **0-35 ngày**
2. Manual Snapshots
    - Snapshots không được tự động hoá 
    - Khi snapshots được tạo sẽ có một sự gián đoạn giữa tài nguyên compute và storage => Chỉ ảnh hưởng khi dùng 1 AZ
    - Manual Snapshots không có thời hạn trừ khi người dùng tự xoá


- Cả hai cách backup trên đều dùng AWS Managed S3 Buckets nên người dùng sẽ không thể thấy 
- Vì backups được sao dùng S3 nên sẽ là region resilience
- Và nếu có MultiAZ thì backups sẽ được sao từ secondary database

#### RDS Read-Replicas

Read-Replicas là một tính năng có thể được dùng với RDS Instance - **5 kết nối trực tiếp với mỗi primary instance**

Read-Replicas dùng **Asynchronous Replication**, nghĩa là sẽ có một độ trễ so với primary, bởi vì dữ liệu sẽ được lưu trước tới local storage của primary database trước khi được sao qua ReadReplica.

ReadReplicas có thể được tạo ở cùng khu vực hoặc khác khu vực với primary. 

ReadReplicas có thể cải thiện hiệu suất, bởi có thể scale read-capacity và có thể cải thiện hiệu suất toàn cầu.

ReadReplicas có thể được đổi thành Primary Instance nếu cần thiết nên có thể giảm RTO xuống. Tuy nhiên ReadReplicas có thể sao data corruption. 

ReadReplicas chỉ được dùng để đọc dữ liệu trừ khi được promote.

### RDS Security

- Dữ liệu in-transit được mã hoá với SSL/TLS và có thể được đặt là mandatory (bắt buộc)
- Dữ liệu at-rest được mã hoá mặc định với EBS volume encryption dùng KMS => Được mã hoá bằng Host hoặc EBS
- Mã hoá không được xoá đi một khi đã khoá.

Ngoài mã hoá mặc định với KMS, RDS còn hỗ trợ Transparent Data Encryption với MSSQL và Oracle. Encryption sẽ được xử lý bởi engine. Oracle còn hỗ trỡ CloudHSM và key sẽ được quản lý bởi người dùng. 

#### IAM Authentication với RDS

- Có thể tạo IAM Authentication để truy cập RDS bằng cách:
1. Tạo một RDS Local DB Account với AWS Authentication Token cho RDS Instance
2. Tạo IAM User và EC2 Role với Policy đính kèm nôid IAM identity đó với local DB user 
3. **generate-db-auth-token** token sẽ được tạo trong vòng **15 phút** và token sẽ được dùng thay cho password

=> IAM chỉ được dùng cho Authentication còn Authorisation được quản lý bởi DB Engine. 

