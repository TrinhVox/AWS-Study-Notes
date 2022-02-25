## Encryption Approaches 
### Encryption At Rest
Encryption At Rest được thiết kế để bảo vệ dữ liệu không bị lấy đi dù maý tính hoặc ổ đĩa bị trộm. 

Vd: máy tính được mã hoá, nghĩa là tất cả dữ liệu được lưu vào máy tính đều được mã hoá. 

Trên đám mây, dữ liệu cũng được mã hoá như vậy, dù dùng chung một kho dữ liệu. 

Encryption At Rest chỉ được sử dụng khi chỉ có một người dùng

### Encryption In Transit 
Encryption In Transit được thiết kế để mã hoá dữ liệu khi được truyền đi. 

Encryption In Transit thường được sử dụng khi có nhiều người dùng dữ liệu. 

## Algorithm:
- Blowfish, AES, RC4, DES, RC5 and RC6

## Encryption Keys
### Symmetric Encryption  - Thuật toán khoá đối xứng 
Symmetric Encryption là khi một chìa khoá được dùng để mã hoá và giải mã dữ liệu. 

1. Hai bên phải đồng ý về một thuật toán để khoá dữ liệu
2. Người gửi dữ liệu phải có chìa khoá và mã hoá dữ liệu bằng chìa khoá đó với thuật toán đã được đồng ý
3. Nguời nhận dữ liệu phải dùng chìa khoá để giải mã dự liệu 

Problem: Làm sao để người gửi gửi chìa khoá cho người nhận một cách an toàn?

=> Symmetric Encryption chỉ nên dùng cho Encryption At Rest

### Asymmetric Encryption - Thuật toán khoá không đối xứng 
Asymmetric Encryption là khi có hai chìa khoá được dùng, một công cộng (public key) và một chìa khoá riêng (private key)

Cả hai bên gửi và nhận đều có 2 bộ chìa khoá, public key và private key.

Public key chỉ được dùng để mã hoá dữ liệu và chỉ có private key mới có thể giải mã dữ liệu đã được mã hoá bằng public key

Thường asymmetric encryption sẽ được dùng lúc đầu để trao đổi encryption key cho symmetric encryption. 

## Signing
Để xác định danh tính người gửi dữ liệu, người dùng có thể mã hoá dữ liệu bằng private key, và người nhận có thể dùng public key của người gửi để xác định dữ liệu được gửi từ đúng người. 




  