## CodeCommit

### Security 
- HTTPS
    + Khi dùng HTTPS để truy cập CodeCommit thì người dùng cần login với username và password
- SSH
    + Nếu dùng SSH để truy cập CodeCommit, người dùng sẽ cần tới Key Pairs

Cả hai cách đều có thể được quản lý bởi IAM 

Người dùng có thể dùng CLI giống git để clone, commit và push lên CodeCommit.

Người dùng cũng có thể dùng Notifications tại CodeCommit để thông báo khi có thay đổi tại repository qua SNS hoặc AWS Chatbot

Hoặc người dùng có thể tạo trigger khi có một event xảy ra trong CodeCommit để invoke Lambda.

### CodePipeline

Để pipeline là dịch vụ dành cho Continuous Delivery. 

CodePipeline có thể điều khiển flow từ source, qua build và tới lúc deploy (triển khai). 

Pipeline được xây dựng dùng stages, và stages được định nghĩa bởi người dùng. 

Stages có Actions và actions có thể xảy ra song song hoặc theo thứ tự. 

