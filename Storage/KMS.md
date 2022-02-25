# Key Management Service

KMS là dịch vụ theo khu vực và là dịch vụ công cộng.

KMS là dịch vụ dùng để tạo, lưu trữ và quản lý các chìa khoá mã hoá. 

KMS có thể dùng cho symmetric và asymmetric

Chìa khoá được tạo ra bởi KMS không bao rời khỏi KMS => tuân thủ theo **FIPS 140-2 (L2)**

KMS được dùng chính để quản lý Customer Master Key (CMK)


## Customer Master Key

CMK bao gồm:
1. ID - dùng để nhận dạng chìa khoá
2. date - ngày chìa khoá được tạo
3. policy - chính sách của chìa khoá (resource policy)
4. description - mô tả 
5. state - trạng thái của chìa khoá (active hoặc inactive)

CMKs có thể dùng để mã hoá dữ liệu tới **4KB**

Khi bắt đầu người dùng sẽ tạo CMK. KMS sẽ mã hoá CMK đã được tạo. Khi người dùng muốn mã hoá dữ liệu, người dùng sẽ gửi dữ liệu cho KMS và KMS sẽ giải mã CMK để dùng CMK mã hoá dữ liệu cho người dùng. Khi người dùng muốn giải mã dữ liệu, người dùng gửi dữ liệu đã được mã hoá cho KMS và KMS dùng CMK đã được giải mã để giải mã dữ liệu cho người dùng. 

Người dùng chỉ được thao tác tuỳ theo quyền được cấp. Ví dụ nếu người dùng chỉ có quyền được mã hoá dữ liệu thì người dùng chỉ được mã hoá dữ liệu và không được giải mã dữ liệu.

Có hai loại CMKs:
1. AWS Managed - là keys được tạo ra bởi dịch vụ của AWS vd như S3
2. Customer Managed - là keys được tạo bởi người dùng

CMKs hỗ trợ thay đổi và xoay các vật liệu dùng để mã hoá dữ liệu mỗi 3 năm với Customer Managed thì rotation là không bắt buộc.

KMS sẽ lưu Backing Key (là vật liệu để mã hoá và giải mã dữ liệu hiện tại) và các Backing Key dùng trước đây những đã được xoay (rotation).

## Data Encryption Key (DEK)

DEK là một loại chìa khoá khác được tạo bởi KMS để mã hoá dữ liệu. 

KMS dùng CMK để tạo ra DEK.

Khi dùng GenerateDataKey để tạo DEK, người dùng có thể mã hoá dữ liệu **trên 4KB**

KMS không giữ DEK, người yêu cầu DEK sẽ giữ DEK. 

Khi tạo DEK, KMS sẽ giao cho người yêu cầu hai loại key:
1. Plaintext version - DEK không được mã hoá
2. Ciphertext version - DEK đã được mã hoá bởi CMK

Người dùng sẽ mã hoá dữ liệu bằng plaintext version và **xoá** plaintext version sau khi dùng để mã hoá.

## Key Policies 
Chính sách cho chìa khoá (resource policy dành cho chìa khoá).

Mỗi CMK đều có một policy. Và nếu là Customer Managed thì người dùng có thể chỉnh sửa policy. 

Sample:

```
{
    "Sid": "Enable IAM User Permissions",
    "Effect": "Allow",
    "Principal": {"AWS": "arn:aws:iam::111122293819:root"},
    "Action":"kms",
    "Resource": "*"
}
```

Key policies được dùng với IAM policies để cho phép IAM users quyền sử dụng CMKs để mã hoá và giải mã dữ liệu. Nhưng có thể người dùng có quyền được trao quyền cho IAM users không có nghĩa là có quyền được dùng CMKs để mã hoá và giải mã dữ liệu. 

Sample IAM Policies để dùng KMS:
```
{
    "Version": "2012-10-17",
    "Statement":{
        "Effect": "Allow",
        "Action":[
            "kms:Encrypt",
            "kms:Decrypt"
        ],
        "Resource":[
            "arn:aws:kms:*:111122293819:key/*"
        ]
    }
}
```

### CLI for encryption and decryption on CloudShell:

```
aws kms encrypt \
    --key-id alias/catrobot \
    --plaintext fileb://battleplans.txt \
    --output text \
    --query CiphertextBlob \
    | base64 --decode > not_battleplans.enc 
    
aws kms decrypt \
    --ciphertext-blob fileb://not_battleplans.enc \
    --output text \
    --query Plaintext | base64 --decode > decryptedplans.txt

```