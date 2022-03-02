
## Containerization
Containers có tác dụng tựa như một máy ảo vì containers tạo ra một environment riêng biệt dành cho ứng dụng. Tuy nhiên containers khác máy ảo ở chỗ, chúng không cần chạy riêng biệt một hệ điều hành, mà chỉ là một hệ thống dùng trong hệ điều hành đó.

Cái lợi của containers là có thể dùng nhiều ứng dụng trên cùng hệ điều hành và chỉ dùng một phần cứng (hardware)

### Docker Image
 Docker Image được tạo ra bởi Dockerfile, mỗi bước trong dockerfile tạo ra một file system layer (lớp), layer trong docker image là readonly. 
 
 Docker Image có thể được tạo ra từ một mẫu cơ sở (base image) hoặc từ file trắng. 

 Một Docker Image có thể tạo ra nhiều Docker Containers. 

### Docker Container
Docker Container được tạo ra bởi Docker Image, nhưng khác ở chỗ Docker Container có thêm một lớp gọi là Read/Write Layer.

## Container Registry
Ví dụ Container Registry là Docker Hub. 



