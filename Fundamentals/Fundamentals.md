# AWS Accounts - Tài khoản AWS

AWS Account is a *container* for **identities** (users) and **resources** (tài nguyên). 

Mỗi tài khoản AWS sẽ có một **root users** và được đăng ký bằng một tài khoản email độc nhất (unique).

Root user có thể truy cập tất cả tài nguyên được tạo bởi tài khoản này và không bị giới hạn (by default).

# Tạo tài khoản:
## Best Practice:
### Management Account (Tài khoản chính)
- MFA
- Billing Alarm
- IAM ADMIN
    Hạn chế dùng quyền lợi của root users bằng cách dùng IAM Admin.

### Production Account 
- MFA
- Billing Alarm
- IAM ADMIN
    Hạn chế dùng quyền lợi của root users bằng cách dùng IAM Admin.

## Multi-Factor Authentication (MFA)
MFA là tính năng được sử dụng để gia tăng bảo mật của tài khoản AWS. 
Khi user đăng nhập tài khoản, ngoài sử dụng tên đăng nhập và mật khẩu, user sẽ phải nhập mã OTP để xác nhận.

Khi tạo MFA cho users:
- AWS sẽ tạo Secret Key và thông tin khác (tên tài khoản, dịch vụ sử dụng,..) và dùng để tạo mã QR
- User dùng ứng dụng trên điện thoại để scan mã và mã OTP sẽ được tạo mỗi 10s

## IAM GROUP, USERS and ROLES Basics
Tất cả IAM identities sẽ được cho phép truy cập toàn bộ hoặc giới hạn truy cập vào tài nguyên được tạo ra bởi tài khoản AWS đang sử dụng.

IAM Identities khi mới được tạo sẽ không có quyền truy cập nào, và chỉ khi được cấp quyền mới được truy cập dịch vụ và tài nguyên đã tạo của tài khoản. 

- IAM is a globally resilience service (ie. All data is always secured across all AWS regions)
### IAM User
Identities which represent human or applications that need access to your account
Cá nhân hoặc ứng dụng cần truy cập tài khoản AWS đã được tạo ra. 
### IAM Group
Nhóm users có cùng công việc liên quan tới nhau
### IAM Roles
Dùng bởi AWS Services, hoặc để trao quyền truy cập cho users của tài khoản khác.

Công dụng chính của **IAM**:
- Quản lý các cá nhân được truy cập tài khoản AWS (ID Provider - IDP)
- Xác thực (Authenticate)
- Cho phép truy cập dịch vụ hoặc tài nguyên của tài khoản (Authorize)
### IAM Access Keys
Access Keys giúp AWS Command Line Tools (CLI Tools) tương tác với tài khoản AWS
Access Keys có thời hạn lâu dài
Mỗi IAM user có **hai** access keys:
- Access Key ID
- Secret Access Key
Access keys có thể được tạo ra, xoá đi hoặc bị tạm ngưng hoạt động
=> Một khi bị mất secret access key, thì sẽ không tìm lại được và sẽ phải tạo một bộ access keys mới và cập nhật CLI Tools với access keys mới. 

```
# check aws CLI version
aws --version

# configure credentials for CLI to interact with AWS Account 
aws configure --profile iamadmin-general
# enter the Access Keys ID and Secret Access Keys
```

# Budgets
Dùng AWS Budgets để tạo ngân sách cho tài khoản và nhận báo động khi tài khoản sử dụng lố ngân sách tạo ra.
