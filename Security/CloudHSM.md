## Cloud HSM 

HSM = Hardware Security Module 

Cần để làm theo tiêu chuẩn FIPS 140-2 Level 3

AWS cung cấp nhưng được quản lý bởi customer

Có thể kết hợp KMS để dùng CloudHSM bằng **custom key store**

HSMs được vận hành bởi AWS ở AWS HSM VPC, và interfaces sẽ được gửi vào VPC của khách hàng. Và AWS CloudHSM Client sẽ được đặt vào EC2 của khách hàng. 

Có thể dùng CloudHSM để offload SSL/TLS cho web server
