## Database Migration Service (DMS )

DMS là dịch vụ quản lý database migration được chạy dùng replication instance. 

Trong mỗi replication instance gồm nhiều replication tasks. Replication task được dùng để tạo bản sao từ Source tới Destination.

Replciation task có thể là Full Load hoặc CDC (Change Data Capture)

DMS migrate không có Downtime

DMS có thể dùng với **Schema Conversion Tool** khi migrate từ hai database engine khác nhau. 

Khi cần migrate dữ liệu với dung lượng nhiều TB có thể dùng kết hợp DMS và Snowball:
1. Dùng SCT để lấy dữ liệu locally và chuyển tới snowball device
2. Gửi device tới AWS, AWS sẽ chuyển dữ liệu tới S3 bucket
3. DMS migrate từ S3 tới storage mong muốn
4. CDC có thể được dùng để lưu những thay đổi

