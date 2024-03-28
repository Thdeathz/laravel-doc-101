# Triển khai ứng dụng Laravel

## Nội dung

| Content                                     |
| :------------------------------------------ |
| [Yêu cầu hệ thống](#server-requirements)    |
| [Cấu hình server](#cấu-hình-server)         |
| [Tối ưu hóa ứng dụng](#tối-ưu-hóa-ứng-dụng) |

## Server Requirements

Để triển khai được một ứng dụng laravel yêu cầu cần cài đặt bản thu gọn PHP và một số extension cần thiết:

- PHP >= 8.2
- Ctype PHP Extension
- cURL PHP Extension
- DOM PHP Extension
- Fileinfo PHP Extension
- Filter PHP Extension
- Hash PHP Extension
- Mbstring PHP Extension
- OpenSSL PHP Extension
- PCRE PHP Extension
- PDO PHP Extension
- Session PHP Extension
- Tokenizer PHP Extension
- XML PHP Extension

## Cấu hình server

### Nginx

Cấu hình cơ bản của Nginx khi muốn triển khai ứng dụng Laravel:

```nginx
server {
    listen 80;
    listen [::]:80;
    server_name example.com;
    root /srv/example.com/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";

    index index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php8.2-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```

## Tối ưng hóa ứng dụng

Ở chế độ production để tăng tốc thì cần thiết phải cached những service hay sử dụng như configuration, events, routes, views. Laravel cung cấp 1 câu lệnh để làm việc đó:

```bash
php artisan optimize
```

Để xóa phiên bản cache cũ thì sử dụng:

```bash
php artisan optimize:clear
```

---

### Caching Configuration

Trong quá trình build ứng dụng, cần thiết phải chạy câu lệnh:

```bash
php artisan config:cache
```

Điều này sẽ combine tất cả các config Laravel file vào một file cached sẽ giảm đáng kể thời gian framework load các biến cấu hình.

---

### Caching events

Cũng nên cache tất cả các events được phát ra tự động bởi Laravel:

```bash
php artisan event:cache
```

---

### Caching routes

Nếu build một ứng dụng laravel lớn với nhiều routes, cần cache chúng:

```bash
php artisan route:cache
```

---

### Caching views

Nếu sử dụng view cache, cần chạy câu lệnh:

```bash
php artisan view:cache
```

---

### Debug mode

Ở chế độ Production cần tắt debug mode bằng cách set biến `APP_DEBUG` sang false trong file `.env`.

---

### The Health Route

Laravel cũng cấp build-in health check route để kiểm tra trạng thái của wungs dụng. Trong production mode, route này có thể được sử dụng để báo cáo trạng thái của ứng dụng cho các hệ thống khác như uptime monitoring, load balancers, k8s, etc.

Mặc định thì route này sẽ phục vụ ở route `/up` và trả về 200 HTTP response nếu ứng dụng hoạt động clear, và 500 HTTP response nếu ứng dụng gặp lỗi. Ta có thể confi URI cho cái route này ở `bootstap/app.php`:

```php
->withRouting(
  web: __DIR__.'/../routes/web.php',
  commands: __DIR__.'/../routes/console.php',
  // health: '/up',
  health: '/status',
)
```

Khi có một HTTP request gọi tới route này, Laravel sẽ phát đi một event `Illuminate\Foundation\Events\DiagnosingHealth`. Bằng cách tạo một `Listener` cho cái event này, ta có thể kiểm tra tất cả các thành phần của ứng dụng và trả về exception tương ứng.
