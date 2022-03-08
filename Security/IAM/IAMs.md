## IAM Identity Policies
IAM Policies là chính sách dành cho mỗi IAM identities (danh tính) để đảm bào độ bảo mật. Mỗi chính sách gồm những statements để cho phép hoặc từ chối truy cập dịch vụ. 

IAM Policy dùng JSON, mỗi statement gồm có:

1. Statement ID (Sid) - để nhận định statement gì (optional)
2. Effect - Allow or Deny (Cho phép hoặc từ chối)
3. Action - Các hoạt động mà IAM identities có thể làm, có thể là hoạt động rất chi tiết hoặc rất rộng. 

    vd: ["s3:*"] => Nghĩa là statement này dành cho tất cả các hoạt động trong S3. 

4. Resources - dành cho tài nguyên của AWS, có thể là tất cả các tài nguyên ("*") hoặc là một list các tài nguyên - ["arn:aws:s3:::catgifs", "arn:aws:s3:::catgifs/*" ]

Sample:

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Sid":"statement1",
         "Effect":"Allow",
         "Action":[
            "s3:*"
         ],
         "Resource":[
            "arn:aws:s3:::*"
         ]
       },
       {
         "Effect": "Deny",
         "Action": "s3:*",
         "Resource": [
           "arn:aws:s3:::catgifs", "arn:aws:s3:::catgifs/*"
         ]
       }
    ]
}
```

Nếu các statements có chính sách không được chia cách rõ ràng, thì sẽ ưu tiên theo thứ tự sau:
1. Explicitly DENY - nếu statement rõ ràng tự chối truy cập tài nguyên và hoạt động trong tài nguyên
2. Explicitly ALLOW - nếu statement rõ ràng cho phép truy cập tài nguyên
3. Default DENY - nếu không có statement rõ ràng là chp phép hoặc từ chối truy cập thì mặc định là từ chối

Inline Policies - Mỗi người dùng, mỗi tài khoản có một chính sách riêng để truy cập một project => một khi chính sách có thay đổi thì phải sửa chính sách cho từng người 

Managed Policies - chính sách chung được trao quyền cho mỗi người => dễ thay đổi hoặc sử dụng hơn nếu project có nhiều người truy cập

## IAM Users 

IAM Users là các idenity (danh tính) được trao quyền truy cập dịch vụ của tài khoản. 


Principal là **một** người hoặc một phần mềm gửi yêu cầu cho IAM để truy cập và sử dụng tài nguyên. 

Authentication là quy trình để một principal xác thực với IAM là mình đúng là người được cấp quyền truy cập và sử dụng tài nguyên. 

Quy trình authentication được thực hiện bằng cách người dùng đưa đúng tên đăng nhập và mật khẩu, hoặc đưa đúng Access Keys (chìa khoá truy cập). 

- Mỗi tài khoản có thể có **5000** IAM Users
- Một IAM User có thể là member của **10** nhóm


## Amazon Resource Name

Amazon Resource Name là tên cho tài nguyên của amazon dùng để xác định tài nguyên trong mỗi tài khoản AWS. 

ARN có định dạng:
```
arn:partition:service:region:account-id:resource-id
arn:partition:service:region:account-id:resource-type/resource-id
arn:partition:service:region:account-id:resource-type:resource-id
```

## IAM Groups

**Containers** cho IAM Users, dùng để sắp xếp và quản lý IAM Users. 

IAM Groups có thể được chia cho các teams trong một project để dễ quản lý và trao quyền. Bởi mỗi group có thể có policies khác nhau.

IAM Groups không có Nesting, nghĩa là không có groups trong groups. 

Có thể có **300** groups trong một tài khoản, nhưng có thể xin thêm qua support.

Groups không phải là một danh tính thật, nên không thể là một principal trong chính sách.

## IAM Roles

IAM Roles là một identity (danh tính) khác có thể được trao quyền trong tài khoản AWS. 

Roles thường được dùng cho **nhiều** người hoặc phần mềm dùng chung tài khoản AWS hoặc tài khoản khác ở ngoài tài khoản của ta. 

Roles thường được dùng để trao quyền **tạm thời**, roles là cách người dùng có thể mượn danh tính để dùng dịch vụ hoặc tài nguyên trong thời gian ngắn. 

Có hai chính sách được dùng với IAM Roles:
1. Trust Policy (Chính sách tin tưởng)
- Dùng để cho phép người dùng nào được quyền dùng IAM Roles để truy cập
- Sau khi cho phép người dùng sử dụng, IAM Roles sẽ tạo tên đăng nhập và password tạm thời cho người dùng 

2. Permisions Policy (Chính sách cho phép)

- Người dùng sử dụng IAM Roles chỉ được sử dụng các dịch vụ và tài nguyên được nêu trong Permission Policy (chính sách cho phép)

### Khi nào dùng IAM Roles?
- AWS Lambda
- Break glass for key - khi cần truy cập cho trường hợp khẩn cấp
- Khi có hơn 5000 người dùng, hoặc khi có rất nhiều người dùng
- Web federation - dùng Google, Facebook để truy cập dịch vụ

## AWS Organisation 
Dùng bởi  các cơ quan để quản lý nhiều tài khoản AWS một cách hiệu quả. 

AWS Organisation có thể được tạo bởi một tài khoản AWS thường, và tài khoản đó sẽ trở thành Management Account (tài khoản quản lý). Tài khoản Management có thể mời các tài khoản khác để vào AWS Organisation đã được tạo. Managment Account cũng sẽ là tài khoản tính tiền cho các tài khoản khác trong tổ chức (consolidated billing)


## Service Control Policies

- JSON documents
- Được đính kèm với Root Container hoặc với Organisation Unit hoặc account 
- Policies này không ảnh hưởng với Management Account
- SCP dùng để giới hạn hoạt động của các tài khoản trong cơ quan
- SCP chỉ tạo giới hạn chứ không được cho quyền

Sample:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        },
        {
            "Effect": "Deny",
            "Action": "s3:*",
            "Resource": "*"
        }
    ]
}
```

## CloudWatch Logs

CloudWwatch logs là dịch vụ công cộng của AWS. CloudWatch dùng để lưu trữ, quản lý và truy cập dữ liệu logging. CloudWatch được kết hợp với nhiều dịch vụ và tài nguyên của AWS. 

CloudWatch là dịch vụ theo khu vực (regional). 

Log stream là một dãy các log events được lấy từ cùng một nguồn

Log groups là một nhóm các log streams từ một loại logging 

Metric filter được dùng ở log groups để tìm xem nếu có logging nào bị lỗi, thì sẽ báo lên metric và trigger alarm

## CloudTrail

CloudTrails là dịch vụ dùng để log các cuộc gọi/hoạt động APIs thành CloudTrail Event

Mặc định CloudTrails sẽ lưu trữ các sự kiện trong vòng **90 ngày** trong Event History (lịch sử sự kiện)

CloudTrails có 2 loại event(sự kiện):
1. Management Event 
2. Data Event

CloudTrails có thể được trail trong 1 khu vực hoặc tất cả các khu vực. 

Sẽ không được lưu trữ sự kiện trong s3 trừ khi tạo ra một trail
 
Trails là cách để cơ cấu S3 và CWlogs để lưu trữ sự kiện 

Mặc định là chỉ có management events

IAM, STS, CloudFront => Global Service Events 

Cloudtrails không phải realtime, mà sẽ bị trễ. 