## Cloudformation 

### Non-portable Cloudformation
Chú ý khi dùng cloudformation:
    - Nếu dùng s3 bucket thì không nên để tên bucket vào properties bởi vì tên bucket phải là độc nhất nên sẽ không thể dùng cùng template tạo ra nhiều bucket cùng tên.
    - Nếu dùng ImageId của AMI để tạo Instance thì phải để ý là AMI Image Id đó chỉ được dùng ở 1 region 

### Pseudo Parameters
Template Parameters cho phép input từ người dùng qua console/CLI/API
Pseudo Parameters dùng parameters từ AWS. Vd: AWS::Stackid

### Intrínic Functions
- **Ref & Fn::GetAtt**: Cho phép reference tài nguyên từ logical resource này qua một resource khác (vd: dùng để reference VPC khi tạo subnet)
=> Dùng !Ref trong template để lấy value của parameters đó. Khi được dùng với logical resource thì physical ID sẽ được trả về
=> !GetAtt được dùng để lấy attribute liên quan tới tài nguyên, thường là để lấy cấu hình. 
- **Fn::Join & Fn::Split**: Cho phép join hoặc split strings
=> Vd:

```
!Join ['', ['http://', !GetAtt Instance, DNSName]]
```
- **Fn::GetAZs & Fn::Select**: Cho phép lấy thông tin AZ và chọn 1 trong list được nhận
=> !GetAZs cho phéo người dùng biết list tên các AZs trong Region.
- **Fn::Cidr**: Cho phép chọn cidr range

### Mappings
Một template có thể có 1 Mapping objects chứa:
    - nhiều mappings
    - để nối keys với values, cho phép sử dụng lookup
=> Có thể dùng !FindinMap để tìm AMI trong region

Vd:

```
# !FindInMap [MapName, TopLevelKey, SecondLevelKey]
!FindInMap ["RegionMap", !Ref 'AWS::Region', "HVM64"]
```

### Outputs

Outputs section trong CloudFormation là optional (không bắt buộc)

Giá trị được nêu trong phần này sẽ:
- có thể được truy cập từ parent stack khi dùng nesting
- có thể được export để cho phép cross-stack references



### DependsOn
Thường CloudFormation sẽ tạo resources song song. 

Tuy nhiên có thể dùng DependsOn để tạo một resource được tạo trước khi tạo resource tiếp theo nếu một resource cần phụ thuộc theo một resource khác. 

Vd: Tạo VPC trước => tạo subnet => tạo Instance trong subnet

### Isolated Stack

Resources trong một stack cô lập sẽ chia cùng một lifecycle và có giới hạn 500 resources trong một stack 

Không thể reference resource từ một stack khác

### Nested Stack
Bắt đầu bằng root stack và parent stack là stack có nested stack (stack lồng vào)

Cloudformation có thể là một resource trong một stack, thì cloudformation resource đó sẽ là một stack được lồng vào stack trước (parent stack)

Khi dùng modular stack thì stack có thể được dùng lại (vd: dùng một template để tạo VPC)

### Cross-stack References

Khi có thể reference một stack khác 

Để dùng thì outputs trong stack phải reference logical resources, và các resources trong stack đều phải có outputs

Và outputs sẽ được export để cho phép các stack khác import vào. 

=> Cross-stack reference nên được dùng khi có nhiều lifecycles và muốn dùng lại resource đã được tạo ra bởi stack khác

### Stack Sets
Cho phép deploy cloudformation stack với nhiều accounts và khu vực khác nhau

StackSets là containers ở trong một admin account và chứa các stack instance (stack instance chứa stacks được tạo ra ở một target account hoặc region được chọn)

Admin account và target accounts phải có trust relationship trước khi dùng stack set để deploy infrastucture. 

Stack set là tài nguyên theo region, nên người dùng sẽ không thể thấy hoặc thay đổi stack set tại một region khác

Có hai cách để trao permissions cho stack set:
1. Self-managed permission: 
- người dùng tạo IAM roles cần thiết để stacksets có thể deploy trên nhiều accounts và regions
2. Service-manged permission:
- người dùng có thể deploy stack instancee tới accounts trong AWS Organizations. Stacksets sẽ tự động tạo các IAM roles cần thiết. Cách này có thể cho phép tự động deploy tại các accounts được thêm vào organization trong tương lai. 

Stack Instance là  một reference cho stack được tạo ở target account. Stack instance có thể hoạt động độc lập ngoài một stack. Ví dụ: nếu stack không thể được tạo vì một lý do nào đó, stack instance có thể hiện lên lý do tại sao stack bị lỗi. 
### Deletion Policy 

Người dùng có thể viết deletion policy cho mỗi resource được tạo ra bởi CFN để chỉ rỏ các actions có thể làm khi logical resource bị xoá khỏi CFN. 

Delete: Khi xoá logical resource là physical resource cũng bị xoá
Retain: Khi xoá logical resource nhưng physical resource được giữ lại 
Snapshot (nếu có hỗ trợ): Khi xoá logical resource có thể tạo snapshot trước khi physical resource bị xoá

### CloudFormation Init
`AWS::CloudFormation::Init` cho phép người dùng nêu tình trạng mong muốn của resource. cfn-init được để vào metadata trong cfn. 

