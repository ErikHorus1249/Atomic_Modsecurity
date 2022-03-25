# Cài đặt Atomicorp Modsecurity Rule
## I. Chuẩn bị cài đặt 
- Vmware workstation 16 pro
- Centos 7-x86_64
- Một tài khoản Atomicorp modsecurity 

## II. Cài đặt
## 1. Cài đặt NginX và Apache server web
Quá trình cài đặt sesrver web thực hiện trên máy riêng biệt, đã cào NginX thì không cài Apache2 và ngược lại

### 1.1. NginX

Thêm EPEL vào Repo 

    $ sudo yum install epel-release
Cài đặt NginX:

    $ sudo yum install nginx

Khởi chạy NginX service

    $ sudo systemctl start nginx

Kiểm tra tình trạng của NginX

    $ sudo systemctl status nginx

![](https://i.imgur.com/ss1EkUF.png)

![](https://i.imgur.com/i6Ox1Mp.png)

### 1.2. Apache2 
Cập nhật httpd 

    $ sudo yum update httpd
Cài đặt httpd:

    $ sudo yum install httpd

Sau khi xác nhận cài đặt, yum sẽ cài đặt Apache và tất cả các phụ thuộc bắt buộc.
Mở cổng cho dịch vụ web.

    $ sudo firewall-cmd --permanent --add-service=http
Nếu cấu hình https thì cần bổ xung lệnh sau cho cổng 443: 

    $ sudo firewall-cmd --permanent --add-service=https

Khởi động lại tường lửa 

` $ sudo firewall-cmd --reload`

![](https://i.imgur.com/0Jj65ej.png)

## 2. Cài đặt ASL WAF

[**ASL**](https://wiki.atomicorp.com/wiki/index.php/ASL_installation#Prerequisites) được thiết kế để tích hợp với hệ điều hành hiện tại của người sử dụng. Các môi trường tùy chỉnh khác với tiêu chuẩn do nhà cung cấp hệ điều hành thiết kế và đóng gói nên tham khảo ý kiến ​​nhóm dịch vụ của chúng tôi để có giải pháp tùy chỉnh.

### 2.1. Cài đặt Database 
Database được yêu cầu cài đật là **mariaDB**

    $ sudo yum install mariadb-server

Kích hoạt dịch vụ Database 
```c
    $ sudo service mariadb start
    or
    $ systemctl enable mariadb
```
### 2.2. Cài đặt ASL
Sư dụng quyền Root trong toàn bộ quá trình cài đặt:

    $ su -

Chạy script cài đặt

    # wget -q -O - https://updates.atomicorp.com/installers/asl |sh
![](https://i.imgur.com/eUqf6BN.png)

Nhấn **Enter** cho đến khi kết thúc điểu khản người dùng( mục này tương đối dài).

Sau khi quá trình cái đặt hoàn tất thực hiện Configure và Upgrade ASL:

![](https://i.imgur.com/X2L5RMB.png)

Trong quá trình configure và upgrade cần phải điển một số thông tin sau đây:
username và password **Atomic user** thông tin này được cấp qua mail xác nhận đăng kí thành công.

![](https://i.imgur.com/YiXj9ud.jpeg)

Thông đang nhập **mariaDB**
- User mặc định: **root**
- Mật khẩu mặc định: None

![](https://i.imgur.com/TBvSUUy.png)

Thông tin địa chỉ email và tần suất gửi mail thông báo từ hệ thống:
- Thêm địa chỉ email của người quản trị
- Chọn thời gian mặc định tần suất gửi mail (mặc định 1 giờ)
- Thêm user có quền SSH vào hệ thống (không phải user **root**)

![](https://i.imgur.com/ICv5s9E.jpeg)

Thông tin về cài đặt và thiết lập luật tường lửa (các port inbound và outbound) có thể enter để hệ thống cài mặc định.

![](https://i.imgur.com/eTiOJp2.png)

Thông tin về kernel virtualization chọn mặc định: **Vmware**

![](https://i.imgur.com/m9YFxB2.png)

Các mục còn lại chọn Default theo gợi ý khi cài đặt, chờ cài đặt hoàn tất thông qua thông báo PASS.
![](https://i.imgur.com/oUzKKNC.png)

Sau khi nhận thông báo truy cập vào ASL web tại: [https://localhost:3000](https://localhost:3000) 
Thì reboot lại server và tiến hành truy cập sau.

![](https://i.imgur.com/ThUvoLx.png)

Đăng nhập bằng tài khản đã được cấp qua mail.

![](https://i.imgur.com/NcTxgHN.png)

Giao diện quả lý sự kiện (events)

![](https://i.imgur.com/ZmvXyEA.png)
![](https://i.imgur.com/hg7BRwO.png)

#### Tham khảo:
- [Digitalocean](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-centos-7)
- [www.atomicorp.com/wiki](https://wiki.atomicorp.com/wiki/index.php/ASL_WAF)
