Mã lỗi

nginx.service: Failed to parse PID from file /run/nginx.pid: Invalid argument
```
vi /usr/lib/systemd/system/nginx.service
[Service]
ExecStartPost=/bin/sleep 1
```


```
systemctl daemon-reexec
systemctl daemon-reload
systemctl restart nginx
```

Tránh race condition khi nginx.pid chưa kịp tạo.

Cho phép nginx ghi log, tạo file hoặc khởi tạo các socket xong trước khi các service khác phụ thuộc vào nó.

Tránh lỗi như: Failed to parse PID from file /run/nginx.pid.
