## Kinesis
- Dịch vụ xử lý và phân tích dữ liệu thời gian thực
- Stream sẽ có nhiều Shards
- Mỗi shards có 1MB Ingestion, 2MB Consumption
- Mỗi shards có thể được giữ trong 24h tới 7 ngày 
### Firehose
- Dùng để đẩy data từ kinesis stream tới storage, hoặc analytic services
- Gần thời gian thực (~60 giây)
- Đẩy tới:
    + HTTP
    + Splunk
    + Redshift
    + ElasticSearch
    + Destination Bucket (S3)
- Có thể được kết nối trực tiếp tới consumer
- Firehose gửi theo mỗi MB hoặc mỗi 60 giây
- Có thể dùng Lambda chung với Firehose để transform dữ liệu 
- Thường dùng để giữ dữ liệu tới Kinesis Stream 

### Data Analytics 
- Dịch vụ phân tích dữ liệu theo thời gian thực dùng SQL 
- Lấy dữ liệu từ Firehose hoặc Streams 
- Destination: 
    - Firehose
    - Lambda
    - Stream
- Nên dùng Data Analytics khi cần phân tích theo thời gian thực 
