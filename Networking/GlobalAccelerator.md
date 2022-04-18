## Global Accelerator

Global Accelerator có 2 anycast IP Adđresses 

Anycast IP's cho phép 1 IP kết nối với nhiều locations. Routing sẽ chuyển traffic tới location gần nhất. 

Traffic sẽ dùng internet và enter vào một Global Accelerator edge location.

Từ edge location, dữ liệu sẽ được chuyển đi trên toàn cầu qua mạng riêng của AWS => ít hops hơn, được quản lý bởi AWS nên sẽ cho performance tốt hơn. 

Global Accelerator có thể dùng với Non HTTP/S (TCP/UDP) - khác với CloudFront 