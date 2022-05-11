## ElastiCache

Database Bộ nhớ (in-memory) dành cho high-performance

Chỉ dùng cho dữ liệu tạm thời (temporary)

Dùng cho REAQD HEAVY workloads với độ trễ thấp

Dùng để chứa Session Data (Stateless Servers)

=> cần thay đổi cách lập trình của ứng dụng để dùng Cache

Hỗ trợ MemcacheD và Redis

### Memcached
Nên dùng Memcached khi:
Dùng cho String, objects
- Cần model đơn giản nhất có thể 
- Cần chạy node lớn với nhiều core và threads
- Cần có tính năng scale-in và scale-out, thêm hoặc bỏ bớt nodes tuỳ theo nhu cầu
- Cần cache objects

### Redis 
=> Cho phép data partition
=> Cho phép encryption tuỳ vào version
=> Cho phép upgrade node type
=> Pub/Sub
=> Backup and restore
=> Sorted sets
=> HA
- Chọn 6.2 - dùng cho data tiering giữa memory và SSD
- 6.0 nếu muốn authenticate users với role
- 5.0 - dùng cho redis stream, cho phép thêm nhiều item mới trong thời gian thực và cho phép blocking/non-blocking
- 4.0 cho phép encryption và thêm hoặc giảm shards 
- 3.2 - thêm hoặc giảm shards 

### Caching Strategy
#### Cache-aside 

Cache-aside còn có tên gọi khác là Lazy-loading. Lazy loading chỉ load dữ liệu vô cache khi cần thiết. 

**Advantages**:
- Chỉ có dữ liệu được yêu cầu mới được lưu lại tại cache - cache sẽ không bị đầy
- Node failure không ảnh hưởng tới ứng dụng

**Disadvantages**:
- Bị thêm delay nếu dữ liệu không được lưu sẵn tại cache
- Stale data - dữ liệu được lưu tại cache không được update 

#### Write-through
Write-through thêm dữ liệu hoặc update dữ liệu vào cache mỗi khi dữ liệu được viết vào database

**Advantages**:
- Dữ liệu trong cache sẽ không bị stale

**Disadvantages**:
- Độ trễ cao
