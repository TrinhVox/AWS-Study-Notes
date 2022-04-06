## X-ray

X-ray là dịch vụ cho phép người dùng theo dõi và phân tích hành vi của các ứng dụng của mình bằng cách cho phép request tracing, exception collection và profiling capabilities. 

Thay vì gửi thẳng dữ liệu tracing tới X-Ray, người dùng có thể gửi JSON segments tới X-ray daemon. 
X-ray daemon được dùng với Linux, Window, macOS, EB và Lambda. 

X-ray daemon chuyên nghe traffic qua UDP port 2000

Có nhiều cách để instrument ứng dụng bao gồm:
- Auto instrumentation - instrument một cách tự động chỉ cần thay đổi cấu hình, thêm auto-instrument agent 
- Library instrumentation - thay đổi một chút trong code của ứng dụng để thêm pre-built instrumentation cho libraries như SQL client, HTTP clients
