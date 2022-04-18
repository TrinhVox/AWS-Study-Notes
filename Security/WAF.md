## Web Application Firewall (WAF)
- Tường lửa cho Layer 7 (HTTP/s)
- SQL Injection, Cross-Site Scripting, Geo Blocks, Rate Awareness
- Luật được thêm vào WEBACL và kết hợp với ALB, API Gateway và Cloudfront


### WAF Rules
- WEBACL Capacity Units - nếu rule đơn giản thì sẽ dùng ít WCU, còn nếu rule complex thì sẽ dùng nhiều WCU
- Sẽ có limit cho số lượng WCU cho phép
- WEBACL có các action mặc định là: ALLOW, BLOCK và COUNT
