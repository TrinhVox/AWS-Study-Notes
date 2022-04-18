## AWS Systems Manager (SSM)

Dịch vụ cho phép người dùng quan sát và quản lý cơ sở hạ tầng AWS và on-premises

Dịch vụ này cần dùng agent - được cài đặt mặc định trên windows và linux AWS AMI

SSM cho phép:
- quản lý inventory và patch assets
- Run commands và manage desired state
- Parameter store - cấu hình và giữ secrets
- Cho phép kết nối tới EC2 một cách an toàn hơn

SSM là dịch vụ public

Để kết nối server on-premises tới SSM: 
- Cần cấu hình managed instance activations:
    + IAM Role
    + Activation Code
    + Activation ID



### SSM Run Command
Run command là tính năng cho phép người dùng chạy 'command documents' để quản lý máy ảo mà không cần quyền truy cập SSH hoặc RDP

Run commands có thể được chạy trên các máy ảo (instances) tuỳ vào tags hoặc resource groups 

Command documents có thể được sử dụng lại và có các parameters

Rate conntrol:
- Concurrency - bao nhiêu máy có thể hoạt động cùng một lúc
- Error threshold - bao nhiêu máy có thể ngưng hoạt động trước khi công việc bị cho là thất bại

Output options - S3 and SNS 

Hoặc kết hợp với EventBridge

Có thể kết nối với SSM từ trong AWS Cloud qua:
- SSM
- EventBridge
- ConsoleUI
- CLI

### SSM Documents
Documents là JSON hoặc YAML documents (giống như cloudformation documents)

Documents được lưu tại SSM Documment Store 

Document được dùng để hỏi về Parameters và thường sẽ bao gồm các bước (steps) để chạy hoặc cấu hình

Command Document được dùng bởi:
- Run Command
- State Manager
- Maintenance Windows

Automation Document được dùng bởi:
- Automation 
- State Manager
- Maintenance Windows

Package Document được dùng bởi 
- Distributor

### Patch Manager

- Patch Baseline - có thể có nhiều patch baseline trong một sản phẩm - dùng để biết cần config gì 
    + Predefined Patch Baseline:
        - Linux: AWS-[OS]DefaultPatchBaseline - explicitly define patches (vd: AWS-AmazonLinux2DefaultPatchBaseline)
        - Window: AWS-DefaultPatchBaseline - Updates cho critical và security
        - AWS-WindowsPredefinedPatchBaseline-OS-Applications - updates cho OS và App
- Patch Groups - nhóm resources trong ssm
- Maintenance Windows - thời gian có thể làm patches (sửa chữa)
- Run command - tính năng căn bản để thực hiện patching
- Compliance - sau khi patch xong, ssm sẽ check compliance của sản phẩm sau khi patch


