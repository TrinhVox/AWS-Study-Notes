## CICD

### Code Pipeline
Thường thì một pipeline sẽ bao gồm những stage:
    1. Code (Lập trình)
        - Github
        - Bitbucket
        => **Code Commit**
    2. Build (Xây dựng)
        - Jenkins
        => **CodeBuild**
    4. Test (Thử nghiệm)
        - Jenkins
        => **CodeBuild**
    5. Deploy (Triển khai)
        - Jenkins
        => **CodeDeploy** 

Để gắn những stage lại với nhau thì có thể dùng **CodePipeline**

### Các file cần nhớ
- Để cấu hình Codebuild thì cần dùng **buildspec.yml**
- Để cấu hình Codedeploy thì dùng **appspec.yml**

### CodeDeploy
Có thể triển khai lên:
- EC2
- Elastic beanstalk
- AWS OpsWorks
- CloudFormation
- ECS
- Service Catalog
- Alexa Skills Kit
- S3




Đọc thêm về appsec và buildspec:
- https://docs.aws.amazon.com/codedeploy/latest/userguide/reference-appspec-file.html
- https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html
