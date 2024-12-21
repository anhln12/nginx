server {
        listen 80 default_server; # Được sử dụng để xử lý mọi yêu cầu không có hostname khớp với bất kỳ


        root /var/www/html;
        index index.html index.htm index.nginx-debian.html;

        server_name _; # **Server name mặc định, áp dụng cho mọi yêu cầu không có tên miền hợp lệ**
        client_max_body_size 100M;
        location / {
            proxy_pass         http://mylocal-fe/;
            proxy_http_version 1.1;
            proxy_set_header   Upgrade $http_upgrade;
            proxy_set_header   Connection keep-alive;
            proxy_set_header   Host $host;
            proxy_cache_bypass $http_upgrade;
            proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header   X-Forwarded-Proto $scheme;
            add_header Content-Security-Policy upgrade-insecure-requests;
            add_header Access-Control-Allow-Origin *;
        }

}



Block các request gọi theo IP Public
```
server {
    listen 80 default_server;
    server_name _; # Server name mặc định, áp dụng cho mọi yêu cầu không có tên miền hợp lệ
server_tokens off;
    return 403; # Trả về mã lỗi 403 Forbidden
}


server {
    listen 443 ssl default_server;
    server_name _;
server_tokens off;
    ssl_certificate /path/to/default.crt;
    ssl_certificate_key /path/to/default.key;
    return 403;
}
```
