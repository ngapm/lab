# Bài Lab
## I/Thay đổi cổng kết nối từ 22 sang 2022
Bạn cần truy cập SSH bằng tài khoản root

` cd /etc/ssh `

Chạy lệnh 

` vim sshd_config `

Tìm mục Port và sửa lại thành Port 2022 sau đó lưu lại file này

## II/Đổi đăng nhập SSH bằng key thay vì bằng mật khẩu

Truy cập tài khoản root thực hiện lệnh

` ssh-keygen `

Các lưu ý sau khi chạy lệnh này
+  Thông tin xác thực lưu ở/home/root/.ssh/id_rsa
+  Public Key lưu ở /home/root/.ssh/id_rsa.pub

Vào file ssh của server và chạy để tắt việc xác thực bằng mật khẩu khi đăng nhập ssh bằng cách chạy lệnh

` sudo vi /etc/ssh/sshd_config `

Tại mục PasswordAuthentication và PermitRootLogin thì bạn sửa lại thành no

+  PasswordAuthentication no
+  PermitRootLogin no

Khởi động lại SSH

` sudo systemctl restart sshd `

Truy cập vào file ssh để copy private key ra file note pad trên máy tính. Thực hiện lệnh sau để vào lấy thông tin private key
 
 ` vi .ssh/id_rsa `
 
Tiếp theo bạn cần thực hiện lần lượt các bước: 
 
 + Save lại file private key trên notepad với đuôi .ppk

 + Tải PuTTyGen về để Import key vừa lưu lại

 + Truy cập PuTTY >>> SSH >>> Auth để Browse đến file Private key trên

 + Vào Session nhập Hostname và Port SSH, nhập tên Session rồi bấm SAVE lại

 + Open file Session vừa mới SAVE

## III/Giới hạn IP kết nối

| Lệnh   | Mô tả     |
| ------------- |:-------------:|
| ` vi /etc/hosts.allow `   | Cho phép IP kết nối   |
| ` vi /etc/hosts.deny `   | Không cho phép IP kết nối       |


## IV/Thay đổi Time Zone

Bạn có thể kiểm tra timezone hiện tại của máy chủ Email bằng cách chạy lệnh là: ` timedatectl `

Để thay đổi timezone hiện tại thì bạn có thể chạy lệnh là: ` sudo timedatectl set-timezone ___  ` Phần __ sẽ được thay thế bằng thời gian mong muốn của bạn

Ví dụ: ` sudo timedatectl set-timezone Asia/Ho_Chi_Minh `

## V/ Cấu hình firewall với UFW để giới hạn các cổng kết nối 

Bạn cần cập nhật firewall trước khi cài đặt bằng cách chạy lệnh 

` sudo apt upgrade –y `

Sau đó bạn có thể kiểm tra trạng thái của Firewall xem đã bật hay chưa ( mặc định là chưa bật ) bằng lệnh 

` sudo ufw status `

Kích hoạt UFW bằng lệnh

` sudo ufw enable `

Bạn có thể cấu hình UFW bằng hai lệnh:

| Lệnh   | Mô tả     |
| ------------- |:-------------:|
| ` sudo ufw allow   `   | Cho phép kết nối   |
| ` sudo ufw deny  `   | Không cho phép kết nối     |

Ví dụ bạn cho phép kết nối cổng tcp bằng port 2022 thì bạn chạy lệnh 

` sudo ufw allow 2022/tcp `

Bạn có thể xem lại các cấu hình và liệt kê theo số thứ tự bằng lệnh 

| Lệnh   | Mô tả     |
| ------------- |:-------------:|
| ` sudo ufw status numbered  `   | Liệt kê các mục theo dạng thứ tự   |
| ` sudo ufw delete numbered  `   | Xóa rule theo số       |

 + Dịch vụ web server HTTP sử dụng cổng 80, mở kết nối bằng lệnh sudo ufw allow http hoặc sudo ufw allow 80 

 + Dịch vụ web server HTTPS sử dụng cổng 443, mở kết nối bằng lệnh sudo ufw allow https hoặc sudo ufw allow 443 

 + Có thể mở kết nối HTTP và HTTPS chỉ theo tên của Web Server: sudo ufw allow 'Apache Full' (nếu máy chủ đang cài web server Apache) hoặc sudo ufw allow 'Nginx Full' (nếu máy chủ đang cài web server Nginx) 

## VI/ Thay đổi tên Hostname

Bạn có thể kiểm tra tên máy chủ hiện tại bằng lệnh ` hostnamectl `

Và bạn có thể chạy lệnh sau ` sudo hostnamectl set-hostname sv48d184.doisonggiadinh.xyz `

## VII/ Cài đặt và cấu hình cơ bản vestacp

Đăng nhập vestacp.com để khởi tạo  theo mẫu có sẵn và có chứa các cấu hình cơ bản như hình bên dưới

![](https://file.matbao.support/system/data/default_home_folder/Hinh/ngapm/msedge_RuYvx6I66A.png)

Vào SSH chạy lệnh ` curl -O http://vestacp.com/pub/vst-install.sh `

Sau khi chạy xong sẽ có thông tin admin control panel và lưu lại thông tin này.

![](https://file.matbao.support/system/data/default_home_folder/Hinh/ngapm/msedge_5vizm06BVZ.png)

Tạo Database trong vestacp. Link tham khảo: https://vestacp.com/docs/

Cài đặt phần mềm hỗ trợ kết nối , cụ thể là đang cài đặt phần mềm FileZilla và đăng nhập tài khoản database

Tải souce wordpress mới nhất tại wordpress.org và giải nén tệp này ở một thư mục nào đó

Mở phần mềm FileZilla và đăng nhập tài khoản database đã khởi tạo trong 

![](https://file.matbao.support/system/data/default_home_folder/Hinh/ngapm/chrome_uDfWGK9J4v.png)

Copy tất cả mã nguồn wordpress đã giải nén trước đó và dán vào thư mục /web/doisonggiadinh.xyz/public_html

![](https://file.matbao.support/system/data/default_home_folder/Hinh/ngapm/chrome_3R5yTyq2P5.png)

![](https://file.matbao.support/system/data/default_home_folder/Hinh/ngapm/rCyiabbw8L.png)

Bạn có thể tham khảo video sau:



https://user-images.githubusercontent.com/111569730/186597934-6ec29151-13ab-44e8-a09d-696f8c222a71.mp4



Truy cập tên miền của mình vào trình duyệt Web sẽ ra giao diện khởi tạo tài khoản quản trị Wordpress và bạn có thể bắt đầu khởi tạo tài khoản của mình.

![](https://file.matbao.support/system/data/default_home_folder/Hinh/ngapm/Teams_iE9CqYsC9v.png)

![](https://file.matbao.support/system/data/default_home_folder/Hinh/ngapm/Teams_mhyEvuk3LG.png)

Chúc bạn thao tác thành công




