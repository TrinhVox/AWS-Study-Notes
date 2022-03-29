## AWS Certificate Manager (ACM)

Được dùng để quản lý certificate dùng cho mã hoá in-transit. 

Có thể tạo, thay mới hoặc triển khai certificates với ACM. 

ACM chỉ hỗ trợ cho dịch vụ của AWS (eg. Cloudfront, ALBs và không hỗ trợ EC2)

Nếu ACM được dùng để tạo certificate thì certificate sẽ được tự động renew (thay mới)

ACM là dịch vụ theo khu vực => Certificate không được rời khu vực đã được tạo hoặc lưu vào

Để dùng certificate với ALB trong khu vực nào đó (vd: ap-southeast-2) thì người dùng cần tạo certificate trong ACM tại khu vực đó

Nếu CloudFront thì cần tạo certificate tại "us-east-1"

