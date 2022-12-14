# LAMP là gì
 LAMP là chữ viết tắt thường được dùng để chỉ sự sử dụng các phần mềm Linux, Apache, MySQL và ngôn ngữ văn lệnh PHP hay Perl hay Python để tạo nên một môi trường máy chủ Web có khả năng chứa và phân phối các trang Web động. Bốn phần mềm nói trên tạo thành một gói phần mềm LAMP.
 # WordPress với LAMP
 * Yêu cầu:

   * Với mục đích của hướng dẫn này, chúng tôi sẽ sử dụng VPS Ubuntu. Lưu trữ VPS Ubuntu của chúng tôi đã được cài đặt sẵn với một ngăn xếp LAMP hoạt động đầy đủ. Tuy nhiên, chúng tôi vẫn sẽ đi qua tất cả các bước cần thiết và hướng dẫn bạn cách tự cài đặt và định cấu hình ngăn xếp LAMP, trong trường hợp bạn đang thực hiện việc này trên một máy chủ sạch.

    * Cũng cần có quyền root SSH đầy đủ hoặc người dùng có đặc quyền sudo. Tất cả các VPS của chúng tôi đều có quyền truy cập root đầy đủ mà không phải trả thêm phí.

    * Tên miền hợp lệ để truy cập trang web WordPress (172.16.222.130)
## Kết nối với máy chủ của bạn và cập nhật hệ thống của bạn
* Trước khi bắt đầu, hãy kết nối với VPS của bạn qua SSH với tư cách là người dùng gốc (hoặc bằng tài khoản quản trị viên) và cập nhật phần mềm hệ thống của bạn lên phiên bản mới nhất hiện có.

* Để kết nối với máy chủ của bạn qua SSH với tư cách là người dùng gốc, hãy sử dụng lệnh sau:
```ssh root@IP_ADDRESS ```
* Mình sẽ dùng `root` với tên `ptson` với địa chỉ `IP_ADDRESS` là `172.16.222.130`
![image](https://user-images.githubusercontent.com/91528234/196365577-8f1470b8-ce85-4e0b-a39a-2e1ed35602a8.png)

* Sau khi đăng nhập, hãy đảm bảo rằng máy chủ của bạn được cập nhật bằng cách chạy các lệnh sau với quyền root:
``` 
# apt update
# apt upgrade 
```
## Cài đặt LAMP
1. Cài đặt Máy chủ Web Apache

* Apache là một máy chủ web nhanh và an toàn và là một trong những máy chủ web phổ biến và được sử dụng rộng rãi nhất trên thế giới. Tính dễ sử dụng của nó làm cho nó rất hấp dẫn khi bắt đầu với máy chủ web và lưu trữ máy chủ web.

* Để cài đặt máy chủ web Apache, hãy chạy lệnh sau:
```
# apt install apache2
```
* Khi quá trình cài đặt hoàn tất, hãy kích hoạt dịch vụ Apache để tự động khởi động khi khởi động hệ thống. Bạn có thể làm điều đó bằng lệnh sau:
```
# systemctl enable apache2
```
* Để xác minh rằng Apache đang chạy, hãy thực hiện lệnh sau:
```
# systemctl status apache2
```
![image](https://user-images.githubusercontent.com/91528234/196366489-10b00c41-46b9-4b76-89e7-0324ef941903.png)
* Bạn cũng có thể mở trình duyệt web và nhập địa chỉ IP máy chủ của mình và thu được page mặc địch của apache như sau:
![image](https://user-images.githubusercontent.com/91528234/196366835-b582859b-8687-41bd-81d3-a7e5a3c60fbb.png)
## Cài đặt MySQL 
* Bước tiếp theo là cài đặt máy chủ cơ sở dữ liệu MySQL sẽ được sử dụng để lưu trữ dữ liệu trên trang WordPress của bạn.

* Để cài đặt máy chủ cơ sở dữ liệu MySQL, hãy nhập lệnh sau:
```
# apt install mysql-server
```
* Trong quá trình cài đặt, bạn sẽ được yêu cầu nhập mật khẩu cho người dùng gốc MySQL. Đảm bảo nhập mật khẩu mạnh.

* Để cải thiện hơn nữa tính bảo mật của cài đặt MySQL của chúng tôi, cũng như thiết lập mật khẩu cho người dùng gốc MySQL của chúng tôi, chúng tôi cần chạy tập lệnh mysql_secure_installation và làm theo hướng dẫn trên màn hình. Chạy lệnh dưới đây để định cấu hình hệ thống của bạn:
```
# mysql_secure_installation
```
* Nếu chương trình yêu cầu bạn nhập mật khẩu gốc MySQL hiện tại, chỉ cần nhấn phím [Enter] một lần, vì không có mật khẩu nào được đặt theo mặc định khi cài đặt MySQL.

* Một số câu hỏi khác sẽ được hiển thị trên màn hình - bạn nên trả lời CÓ cho tất cả chúng bằng cách nhập ký tự ‘Y’:
![image](https://user-images.githubusercontent.com/91528234/196368110-2d06b199-d453-4b61-9f12-eb2277cda27f.png)
* Bạn cũng sẽ cần kích hoạt MySQL để bắt đầu khởi động với điều này:
```
# systemctl enable mysql
```
## Cài đặt PHP
* Bước cuối cùng của quá trình thiết lập ngăn xếp LAMP của chúng tôi là cài đặt PHP. WordPress là một CMS dựa trên PHP, vì vậy chúng tôi cần PHP để xử lý nội dung động của trang WordPress của chúng tôi.

* Ubuntu 20.04 đi kèm với PHP 7.4 theo mặc định. Chúng tôi cũng sẽ cần một số mô-đun bổ sung để cho phép PHP kết nối và giao tiếp với các phiên bản Apache và MySQL của chúng tôi. Để cài đặt PHP cùng với các mô-đun MySQL và Apache được yêu cầu, hãy chạy lệnh sau:
```
# apt install php libapache2-mod-php php-mysql
```
* Để xác minh rằng PHP 7.4 đã được cài đặt thành công, hãy chạy lệnh sau:
```
php -v
```
* Bạn sẽ nhận được kết quả sau trên màn hình của mình:
![image](https://user-images.githubusercontent.com/91528234/196369617-4aeb2061-479e-45b4-ad67-f2744bb57e58.png)
## Cài đặt WordPress

* Bây giờ chúng ta đã thiết lập xong môi trường LAMP, bây giờ chúng ta có thể tiến hành cài đặt WordPress. Trước tiên, chúng tôi sẽ tải xuống và đặt các tệp cài đặt WordPress trong thư mục gốc của tài liệu máy chủ web mặc định `/var/www/html`.

* Bạn có thể di chuyển đến thư mục này bằng lệnh sau:
```
cd /var/www/html
```
* Bây giờ chúng ta có thể tải xuống bản cài đặt WordPress mới nhất bằng lệnh sau:

```
# wget -c http://wordpress.org/latest.tar.gz
```
* Sau đó, giải nén các tệp bằng:
```
# tar -xzvf mới nhất.tar.gz
```
* Các tệp WordPress được giải nén bây giờ sẽ được đặt trong thư mục wordpress tại vị trí sau trên máy chủ của bạn `/var/www/html/wordpress`

## Tạo cơ sở dữ liệu cho WordPress
* Tiếp theo, chúng tôi sẽ tạo cơ sở dữ liệu và người dùng MySQL cho trang WordPress của chúng tôi. Đăng nhập vào máy chủ MySQL của bạn bằng lệnh sau và nhập mật khẩu gốc MySQL của bạn:
```
# mysql -u root -p
```
* Để tạo cơ sở dữ liệu mới cho cài đặt WordPress của chúng tôi, hãy chạy các lệnh sau:
```
CREATE DATABASE wordpress_db;
CREATE USER wordpress_user@localhost IDENTIFIED BY 'strong-password';
GRANT ALL PRIVILEGES ON wordpress_db.* TO wordpress_user@localhost;
FLUSH PRIVILEGES;
exit;
```
* Bạn có thể thay thế tên cơ sở dữ liệu (wordpress_db) và tên người dùng MySQL (wordpress_user) bằng tên riêng của bạn nếu bạn muốn. Ngoài ra, hãy đảm bảo thay thế “strong-password” bằng một mật khẩu mạnh, mình dùng ở đâu là `p@ssw0rd`.
![image](https://user-images.githubusercontent.com/91528234/196373447-fc96b50a-ed3c-442e-a1c0-9f6f5835d6f3.png)


* Khi cơ sở dữ liệu được tạo, chúng tôi sẽ cần thêm thông tin này vào tệp cấu hình WordPress.
* Đảm bảo bạn đang ở đường dẫn sau `/var/www/html/wordpress` :
```
# cd /var/www/html/wordpress
```
* và sau đó chạy lệnh sau để đổi tên tệp cấu hình mẫu:
```
# mv wp-config-sample.php wp-config.php
```
* Bây giờ, hãy mở tệp wp-config.php bằng trình soạn thảo văn bản yêu thích của bạn, ví dụ:
```
# vim wp-config.php
```
* Và cập nhật cài đặt cơ sở dữ liệu, thay thế wordpress_db, wordpress_user và strong_password bằng các chi tiết của riêng bạn:
```
// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */define('DB_NAME', 'wordpress_db');

/** MySQL database username */define('DB_USER', 'wordpress_user');

/** MySQL database password */define('DB_PASSWORD', 'strong-password');

/** MySQL hostname */define('DB_HOST', 'localhost');

/** Database Charset to use in creating database tables. */define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */define('DB_COLLATE', '');
```
* Mình thay `strong-password` bằng `p@ssword`
![image](https://user-images.githubusercontent.com/91528234/196385521-30ead2cd-d59b-491c-a977-0f93525cdd43.png)

## Cấu hình máy chủ Apache


* Di chuyển đến file sau:
```
# /etc/apache2/site-availble
```
* mở file sau bằng trình soạn thảo Vim:
```
# vim 000-default.conf 
```
* Sửa file như sau
![image](https://user-images.githubusercontent.com/91528234/196387312-d500116d-79ae-4af2-ab3d-d990b1a31475.png)
* Reset lại apache server
```
# systemctl restart apache2
```

![image](https://user-images.githubusercontent.com/91528234/196387489-fedea10c-d29c-49b9-9928-a5cb16cd8805.png)
Chúc mừng bạn đã hoàn thành việc cài đặt và trải nghiệm thôi :3


## Tham khảo thêm tại
* https://www.rosehosting.com/blog/how-to-install-wordpress-with-lamp-stack-on-ubuntu-20-04/#:~:text=The%20LAMP%20stack%20is%20a,MySQL%20database%20server%20and%20PHP.
* https://www.youtube.com/watch?v=q-qfLUTgUl8&t=302s&ab_channel=TonyTeachesTech

