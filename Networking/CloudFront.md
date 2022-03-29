## CloudFront
CloudFront là dịch vụ dành cho Content Delivery Network của AWS. 

CloudFront chỉ hỗ trợ READ Only => Download 

### CloudFront Terms
- Origin - Vị trí nguồn của nội dung
- Distribution - Đơn vị cấu hình của Cloudfront
- Edge Location - bộ nhớ cache gần nhất tại khu vực của dữ liệu 
- Regional Edge Cache - Phiên bản lớn hơn của Edge Location 


### Cloudfront Behaviours

Khi có kết nối tới Edge Location, sẽ được so sánh để xem có khớp với behaviours đã được đặt sẵn không (vd. Default-*)

Có thể hạn chế quyền truy cập => Trusted Signers (người được truy cập)

Các Caching controls được đặt trong behaviours

### TTL 

TTL xử lý khi objects hết hiệu lực cache (expire) ở Edge location.

Khi object được cache hết hạn sử dụng, khi có request từ client, edge location sẽ forward (chuyển tiếp) request tới Origin:
    1. Nếu object không có phiên bản mới - **304 Not Modified** được trả về Edge Location và object đã được lưu sẽ được gửi tới client
    2. Nếu object có phiên bản mới - **200 OK** với phiên bản mới được trả về edge location và gửi tới client   

Edge Location xem object là valid khi objet vẫn còn trong TTL. TTL mặc định là **24 giờ** (validity period)

Users có thể đặt minimum TTL và maximum TTL

Có nhiều header được gắn với Object để đặt TTL:
- Cache-Control max-age (seconds)
- Cache-Control s-maxage (seconds)
- Expires (Date & Time)

Header có thể được đặt bởi S3 hoặc custom origin


### CloudFront and SSL

CloudFront có Default Domain Name - CNAME (Tên miền mặc định) vd: https.//__.cloudfront.net/

SSL được hỗ trỡ theo mặc định cho Default Domain Name

Khi cần Alternate Domain Names - CNAME (Tên miền khác) thì user cần xác minh chủ quyền bằng certificate cho tên miền đó. 

Có thể tạo tên miền qua ACM tuy nhiên chỉ có thể tạo trong khu vực **us-east-1**

Cloudfront chỉ có thể dùng public certificates (không phải self-signed)

SNI là tính năng bổ sung thêm cho TLS để có thể thêm host trong TLS và hỗ trợ dùng nhiều hosts trong cùng IP 

### Securing Content Delivery with CloudFront 

Origin Access Identities là tính năng để tạo danh tính ảo với CloudFront Distribution và triển khai tới Edge location. 

Quyền truy cập tới S3 bucket có thể được quản lý vởi OAI - cho phép truy cập từ OAI và implicit DENY cho tất cả các thứ khác. 

Với Custom Origin, users có thể dùng Customer Header. Origin bắt buộc phải có Header, và Header chỉ được gắn qua Edge location. Hoặc có thể dùng tường lửa cho IP Ranges được dùng bởi CloudFront

### Lambda@Edge

Lambda@Edge là tính năng để users có thể chạy Lambda tại edge location. 

Lambda@Edge Use Cases:
- A/B testing
- Migration Between S3 Origins
- Different Objects Based on Device
- Vary content by country

### Private Distributions
Public - là khi người dùng có thể truy cập tất cả objects qua cloudfront
Private - là khi người dùng yêu cầu truy cập cần phải có Signed Cookie hoặc URL

Có thể có cả hai public và private bằng cách dùng nhiều behaviours

Để cho phép private distribution:
- Root user phải có Cloudfront Key và account đó được thêm vào Trusted Signer

Signed URL cho phép truy cập **1 object** và cho legacy distributions

Signed cookies cho phép truy cập tới nhiều nhóm objects 

### Cloudfront Geo-restriction 

Nếu cho phép Geo-restriction, cloudfront sẽ không cho phép người dùng ở khu vực nào đó truy cập

Có hai mode:
- whitelist 
- blacklist

Cả hai mode đều chỉ cho phép dùng theo nước (country) qua country code

Nếu muốn dùng restriction nhiều hơn country code thì có thể dùng 3rd party geo Location architecture

### Field-level Encryption

Cho phép cloudfront mã hoá thông tin tại Edge location.

Cloudfront mã hoá thông tin tại edge location dùng public key. Người dùng có thể cấu hình loại thông tin nào muốn được mã hoá. Và thông tin được mã hoá sẽ được truyền cùng với các thông tin khác qua HTTPS tunnel. chỉ có thể dùng private key để giải mã thông tin. 

Field-level Encryption nên được dùng cho các industry có nhiều thông tin sensitive => y tế
