## Cognito

Cognito là dịch vụ dành cho Authentication, Authorization và User Management cho ứng dụng

### USER POOLS
- Dùng để users đăng nhập và lấy JSON Web Token (JWT)
- Có thể dùng các identity providers khác giống như Google, Facebook, Amazon hoặc Apple
- Dùng để dùng self-managed resources
- Dùng để truy cập APIs
- Tất cả người dùng (members) đều có directory profile có thể truy cập qua Software Development Kit (SDK)
-  Users pool được dùng cho authentication (identity verification)

### IDENTITY POOLS 
- Dùng để cho phép người dùng truy cập bằng AWS Credentials tạm thời
    - Unauthenticated Identities - cho phép Guest Users
    - Federated Identities - Dùng Google, FB, Twitter để có AWS Credentials tạm thời để truy cập AWS resources
- Identity pools được dùng cho authorization (access control)

