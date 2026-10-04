# BT03 - Cài đặt Nginx và cấu hình Website tĩnh cơ bản (Basic Nginx Web Server)

## 1. Cài đặt Nginx từ repository mặc định

```bash
devops@0310263186:~$ sudo apt update
devops@0310263186:~$ sudo apt install -y nginx
devops@0310263186:~$ nginx -v
nginx version: nginx/1.24.0 (Ubuntu)
devops@0310263186:~$ sudo systemctl status nginx --no-pager
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; preset: enabled)
     Active: active (running) since Sun 2026-10-04 14:40:03 UTC; 12s ago
```

## 2. Tạo thư mục mã nguồn và file index.html

```bash
devops@0310263186:~$ sudo mkdir -p /var/www/ptit-web/html
devops@0310263186:~$ sudo nano /var/www/ptit-web/html/index.html
```

Nội dung `/var/www/ptit-web/html/index.html`:

```html
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <title>PTIT DevOps</title>
</head>
<body>
    <h1>Welcome to PTIT DevOps Course - Session 02</h1>
</body>
</html>
```

## 3. Tạo Server Block cho trang web

File `/etc/nginx/sites-available/ptit-web.conf`:

```nginx
server {
    listen 80;
    listen [::]:80;

    server_name _;

    root /var/www/ptit-web/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

## 4. Kích hoạt Server Block và hủy cấu hình default

```bash
devops@0310263186:~$ sudo ln -s /etc/nginx/sites-available/ptit-web.conf /etc/nginx/sites-enabled/
devops@0310263186:~$ sudo rm -f /etc/nginx/sites-enabled/default
devops@0310263186:~$ ls -la /etc/nginx/sites-enabled/
total 8
drwxr-xr-x 2 root root 4096 Oct  4 14:42 .
drwxr-xr-x 8 root root 4096 Oct  4 14:40 ..
lrwxrwxrwx 1 root root   45 Oct  4 14:42 ptit-web.conf -> /etc/nginx/sites-available/ptit-web.conf
```

## 5. Kiểm tra cú pháp và reload Nginx

```bash
devops@0310263186:~$ sudo nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
devops@0310263186:~$ sudo systemctl reload nginx
```

## 6. Kiểm tra truy cập

```bash
lephu@local:~$ curl -i http://160.30.160.103
HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)
Date: Sun, 04 Oct 2026 14:43:20 GMT
Content-Type: text/html
Content-Length: 187
Last-Modified: Sun, 04 Oct 2026 14:41:55 GMT
Connection: keep-alive

<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <title>PTIT DevOps</title>
</head>
<body>
    <h1>Welcome to PTIT DevOps Course - Session 02</h1>
</body>
</html>
```
