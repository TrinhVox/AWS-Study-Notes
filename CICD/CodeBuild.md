## CodeBuild 

CodeBuild là dịch vụ Code build as a service. 

Người dùng chỉ trả phí cho resources (tài nguyên) được sử dụng trong lúc build. 

Có thể dùng tương tự với Jenkins để build và test. 

CodeBuild dùng docker cho build environment, và có thể được customised (tuỳ chỉnh).

Để cấu hình hoặc tuỳ chỉnh CodeBuild, người dùng có thể chỉnh file buildspec.yml và vận hành trong root của source code. 

CodeBuild cũng có thể lưu logs vào S3 và CloudWatch Logs. 

### Buildspec

Buildspec.yml gồm có 4 Phases chính 
- Install - để install các packages vào build environment
- Pre_build - để sign-in hoặc install dependencies
- Build - dùng để chạy code để build
- Post_build - dùng để package và đẩy các docker image đã được tạo đi 
- Artifacts - các file được dùng trong code
