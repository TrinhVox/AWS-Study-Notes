## Event-Driven Architecture

### Monolithic

Kiến trúc monolithic bao gồm tất cả các phần trong một system sẽ được đặt chung với nhau (Vd: Kiến trúc của Youtube, thì các phần Upload, Processing, Store and Manage đều được vận hành cùng nhau). Điều này có nghĩa là nếu có lỗi xảy ra thì tất cả sẽ đều fail. 

Kiến trúc monolithic cũng có nghĩa là phần sẽ được scale cùng nhau và chỉ được scale vertically. 

Kiến trúc monolithic cũng nghĩa là phí sẽ được tính cùng nhau. 

### Tiered Architecture

Kiến trúc tiered nghĩa là các phần sẽ được chia thành từng tầng riêng biệt và có thể ở cùng server hoặc trên nhiều server khác nhau. 

Nghĩa là mỗi tầng có thể scale độc lập với các tầng khác horizontally và vertically. 


### Queues

Queues là hàng để các messages xếp vào theo thứ tự khi được kết nối. Queus là tính năng được đặt vào giữa các tầng để cho phép các tầng làm việc riêng biệt mà không cần chờ đợi câu trả lời từ tầng tiếp theo (asynchronous). Vd: Youtube:
- Sau khi client đã upload xong video thì video sẽ được lưu vào S3
- Cùng lúc đó message sẽ được thêm vào queue để gửi cho tầng Processing tiếp theo về thông tin để xử lý 
- Sau khi đã gửi message tầng upload có thể tiếp tục xử lý các yêu cầu khác thay vì phải chờ cho Processing trả lời

Queue có thể được dùng chung với ASG để ASG có thể scale tuỳ vào số lượng message trong queues. 

### Microservice Architecture

Microservice có thể được xem giống như là một ứng dụng độc lập. Và kiến trúc Microservice là khi các ứng dụng microservice được kết nối bằng queues để hoạt động chung với nhau.

Thay vì phải kết nối nhiều queues với nhau khi có nhiều microservices, có thể dùng event router để chỉ định các queues. 

