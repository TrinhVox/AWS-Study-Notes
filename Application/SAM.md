## AWS Serverless Application Model (SAM)
SAM là một open-source framework dùng để tạo ứng dụng serverless trên AWS. 

Ứng dụng serverless kết hợp các functions Lambda, nguồn events, và các tài nguyên khác với nhau để thực hiện tasks. Ứng dụng serverless có những tài nguyên khác Lambda như APIs, databasess, và event source mappings. 

=> Bổ sung từ CloudFormation

Template của SAM giống với CloudFormation. Các điều khác nhau giữa SAM với CloudFormation là:
- Transform declaration ` Transform: AWS::Serverless-2016-10-31 ` là bắt buộc trong SAM file. 
- Globals là phần chỉ có ở SAM để định nghĩa các propoerties được dùng chung bởi các function serverless. 