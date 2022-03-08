### Aurora

Aurora dùng **Cluster**. 1 Cluster gồm có 1 primary instance + 0 hoặc nhiều replicas (bản sao). Replicas có thể dùng để đọc dữ liệu. 

Instance trong cluster của aurora không dùng local storage mà dùng **cluster volume** 

Aurora có thể có tới 15 replicas để dự phòng khi failover

Cơ sở dữ liệu sẽ được tính phí theo lượng sử dụng của người dùng, và tất cả đều là SSD Based (IOPS cao, độ trễ thấp)

Replicas có thể được thêm vào hoặc lấy ra mà không ảnh hưởng đến storage. 

Aurora không có free-tier và không hỗ trợ cho Micro Instance (1 máy). 

Cho mỗi compute (điện toán) được dùng, phí sẽ tính theo giờ, mỗi giây và ít nhất 10 phút. 

Cho storage được dùng, phí sẽ được tính theo GB-tháng được dùng, và phí IO mỗi yêu cầu (request). Thường phí backup đã được tính vào. 

Restores sẽ tạo cluster mới. 

Aurora còn có thể backtrack để quay lại thời điểm chưa có lỗi ở cùng một cluster mà không cần tạo cluster mới. 

#### Aurora Serverless
Aurora Serverless dùng Aurora Capacity Units. Nghĩa là người dùng sẽ chỉ định max và min ACU cho cluster và cluster sẽ tự điều chỉnh theo. 

Lượng tiêu dùng được tính phí theo giây

ACU được quản lý bởi AWS và sẽ được share bởi nhiều người dùng

Aurora Serverless thường được dùng bởi:
- Ứng dụng không sử dụng thường xuyên
- Ứng dụng mới - không biết được capacity cần thiết
- Khối lượng công việc thay đổi
- Khối lượng công việc không thể đoán trước
- Dùng để develop hoặc test databases
- Ứng dụng multi-tenant

#### Aurora Global Database
Tính năng của Aurora Provisioned cluster dùng để tạo replicas ở nhiều khu vực khác nhau để cải thiện RPO và RTO cho Disaster Recovery (DR) và Business Continuity (BC)

Aurora global Database được dùng khi:
 - DR và BC ở nhiều khu vực
 - Scale read capacity toàn cầu - cải thiện độ trễ
 - **1s** để sao giữa các khu vực 
 - Có thể có Maximum 5 khu vực secondary
 - Khu vực secondary có thể có 16 bản sao 



#### Aurora Multi-Master

Tính năng của Aurora Provisioned cluster dùng để cho phép nhiều Instance thực hiện đọc và viết cùng một lúc - thay vì chỉ có 1 instance được viết. 

=> Multi-Master có thể hỗ trợ Fault-Tolerance trong khi Single-Master chỉ có thể hỗ trợ High-Availability


