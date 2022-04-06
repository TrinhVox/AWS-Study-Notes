## ElastiCache

Database Bộ nhớ (in-memory) dành cho high-performance

Chỉ dùng cho dữ liệu tạm thời (temporary)

Dùng cho REAQD HEAVY workloads với độ trễ thấp

Dùng để chứa Session Data (Stateless Servers)

=> cần thay đổi cách lập trình của ứng dụng để dùng Cache

Hỗ trợ MemcacheD và Redis

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
