## AWS Single Sign-on (SSO)

SSO có cho phép nguồn nhận dạng (identity source) linh hoạt (flexible)

1. Built-in identity Store

2. AWS Managed Microsoft AD

3. On-premises Microsoft AD (Cần two-way trust hoặc AD connector)

4. External Idenity Provider - SAML 2.0

### Architecture

SSO có thể được tích hợp với Active Directory trên on-premises. 

SSO cho phép Single-Sign on qua SSO Identity Source và quản lý permission tại một điểm tập trung (centralised) cho nhiều tài khoản khác nhau. 

Requirement cho SSO là phải có AWS Organisation

Khi tạo SSO, SSO sẽ thêm các IAM roles cần thiết vào các tài khoản trong Organisation để cho phép quyền truy cập cho SSO

Người dùng sẽ tạo một User portal URL để log in (vd: midway)

Người dùng cũng có thể có phép MFA tại SSO

