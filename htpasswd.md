Khi muốn thiết lập mật khẩu truy cập (basic auth) cho ứng dụng chạy qua Nginx, bạn có thể sử dụng HTTP Basic Authentication.

1. Cài công cụ tạo file mật khẩu
```
apt install apache2-utils -y
yum install httpd-tools -y
```
2. Tạo file chứa tài khoản đăng nhập
Ví dụ: tạo 1 user admin với mật khẩu được mã hóa lưu trong /etc/nginx/.htpasswd
```
htpasswd -c /etc/nginx/.htpasswd admin
```
- Tùy chọn -c tạo mới file (chỉ dùng lần đầu)
- Nhập password khi được hỏi

3. Cấu hình Nginx bảo vệ khu vực
```
server {
    listen 80;
    server_name yourdomain.com;

    location / {
        proxy_pass http://localhost:8080;
    }

    location /phpmyadmin {
        auth_basic "Restricted Area";
        auth_basic_user_file /etc/nginx/.htpasswd;

        proxy_pass http://localhost:8081; # ứng dụng thực tế
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

4. Kiểm tra truy cập
- Khi bạn truy cập link: http://yourdomain.com/phpmyadmin
- Trình duyệt sẽ hiển thị hộp thoại yê cầu nhập username/password
