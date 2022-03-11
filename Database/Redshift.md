## Redshift

Dịch vụ Data Warehouse dùng để phân tích dữ liệu

Hỗ trợ Petabyte

Redshift hỗ trợ OLAP (Online Analytical Processing) dùng để phân tích dữ liệu theo cột trong thời gian dài

Có thể query trực tiếp qua S3 dùng Redshift Spectrum, hoặc trực tiếp query qua nhiều databases khác nhau dùng federated query

Redshift là server based

Chỉ dùng trong 1 AZ

Leader Node - Query input, planning và aggregation

Compute Node - performing queries of data

Có thể dùng Enhanced VPC Routing

Có backups tự động tới S3 (8hours, 5GB) - 1 day retention by default

Có thể có manual backups và chỉ bị xoá khi người dùng muốn

- Có thể dùng với:
    + DMS 
    + S3
    + DynamoDB 
    + Firehose

### Resilience 
- Có thể copy snapshots qua region khác
