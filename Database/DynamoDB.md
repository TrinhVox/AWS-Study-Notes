## DynamoDB

Dịch vụ Database NoSQL - Dùng cho Key/Value và Document 

Được tự động hoá để scale in/out 

Regional Resilient và có thể là global

Rất nhanh bởi vì là SSD

Có thể action theo events

Một Table trong DynamoDB là một nhóm Item (row) với cùng Primary Key

Primary Key phải là unique (độc nhất) và có thể gồm:
1. Partition Key (Simple) - chỉ duy nhất Partition Key 
2. Sort Key (Composite) - có thể dùng Sort Key + Partition Key để tạo Primary Key

MAX mỗi Item = **400KB**

Tốc độ được đặt cho mỗi table:
- Writes (Viết) 1 WCU = **1KB** trên giây
- Reads (Đọc) 1 RCU = **4KB** trên giây

### Backups

#### On-Demand Backups 
- Luôn có một bản sao đầy đủ của Table được lưu cho tới khi bị xoá đi bởi users
- Có thể dùng bản backups này để restore (khôi phục) một table mới tại region mới hoặc đổi settings mã hoá. 

#### Point-in-time Recovery 
- Bản sao sẽ được lưu lại liên tục để cho phép quay lại mọi thời điểm trong một khung giờ 
- Khung giờ là 35 ngày 
- Có thể khôi phục thành một table mới trong 1 giây

### Mode

#### On-Demand
- Mode này được sử dụng khi không biết được lượng dữ liệu, không thể tính trước và có ít quản trị 
- Chỉ trả phí có mỗi 1 triệu operation (viết/đọc)

#### Provisioned
- RCU và WCU được đặt ra cho mỗi table 
- 1 RCU = 1x4KB read operation per second
- 1 WCU = 1 KB write operation per second
- Mỗi table có RCU và WCU burst pool (300 seconds)

### Operations

#### Query
- Query chấp nhận 1 PK value và có thể 1 SK. 
- Capacity (dung lượng) được tính là số lượng tất cả items được trả về

#### Scan
- Scan có thể nhìn qua cả table theo từng item. 
- Có thể dùng tất cả attributes để filter
- Tính phí cho mỗi item được scan qua

### Read Consistency 

#### Eventual Consistent
- Rẻ hơn Strong Consistent 50%
- Khi read, user có thể được dẫn tới 1 node trong tất cả các node, có thể node đó chưa được update.

#### Strong Consistent
- Lúc nào cũng dùng Leader Node nên luôn luôn đúng
- Tuy nhiên vì chỉ dùng 1 node, nên sẽ không thể scale

### Calculation 

#### WCU Calculation
=> Cần viết 10 ITEM/s - 2.5K size mỗi Item 
1. WCU mỗi Item => Item size/1KB => Round Up = 2.5/1 = 2.5 => **3**
2. Nhân với trung bình items mỗi giây => (1)*ITEM/s = 3x10 => **30**
#### RCU Calculation 
=> Cần đọc 10 ITEM/s - 2.5k size mỗi item 
1. RCU mỗi Item => Item size/4KB => Round Up = 2.5/4 = 0.625 => **1**
2. Nhân với trung bình items mỗi giâu => (1)*ITEM/s = 1x10 = **10**

### Indexes
Indexes là tính năng để lấy dữ liệu ra một cách hiệu quả
#### Local Secondary Indexes (LSI)
Dùng để tạo một index khác dùng Sort Key
- Phải được tạo cùng với table
- Max **5 LSI** mỗi table
- Dùng RCU và WCU giống với table
- Có thể chọn attributes - ALL, KEY ONLY & INCLUDE
- Chỉ có item có value mới được thêm vào index
#### Global Secondary Indexes (GSI)
Dùng để tạo index với PK và SK khác
- Có thể tạo lúc nào cũng được 
- Giới hạn mặc định là **20**
- Có RCU và WCU riêng
- Cũng là sparse nên chỉ Item có value mới được thêm vào index
- Eventual consistency 

### Streams
- Stream là một list được xếp theo thời về thay đổi của Item trong table
- Rolling window 24giờ   
- Được cho phép cho mỗi table 
- Ghi lại Inserts, updates, và deletes
- View types:
    + KEYS_ONLY => PK/SK của ITEM được thay đổi
    + NEW_IMAGE => Item sau khi được đổi
    + OLD_IMAGE => Item trước khi được đổi
    + NEW_AND_OLD_IMAGES => Item trước và sau khi thay đổi mà không cần dùng table chính
#### Triggers
- Khi Item được thay đổi sẽ tạo ra một event 
- Event sẽ chứa dữ liệu được thay đổi 
- Action được thực hiện dùng dữ liệu đó
=> Streams + Lambda

### Global Tables
- Cho phép có multi-master cross-region replication
- Được tạo bằng cách:
    + Tạo nhiều tables ở nhiều regions khác nhau và nối các tables với nhau thành globle table 
- Last Writer Wins - nếu hai table có viết cùng lúc thì sẽ chọn write gần nhất là cái đúng
- Chỉ là strongly consistent read trong cùng khu vực được write


### Accelerator (DAX)
Tính năng bộ nhớ cache thiết kế riêng cho DynamoDB 
- Dùng DAX SDK được gắn vào Application
- Application chỉ call một lần còn DAX sẽ là người retrieve dữ liệu
- DAX hoạt động trong một VPC 
- Có một node chính được dùng để read và write còn những node phụ là node replica được để ở AZ khác 
- Hỗ trợ Write-through, dữ liệu được viết vào DynamoDB và DAX cùng lúc thông qua DAX SDK
=> Không nên dùng cho Strongly Consistent

Có 2 cache DAX giữ:
- Item cache - giữ kết quả từ Batch(GetItem)
- Query cache - giữ dữ liệu dựa trên query/scan parameters