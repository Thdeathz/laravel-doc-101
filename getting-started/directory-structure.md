# Cấu trúc thư mục của một ứng dụng Laravel

## Nội dung

| Content                       |
| :---------------------------- |
| [Thư mục root](#thư-mục-root) |
| [Thư mục app](#thư-mục-app)   |

## Thư mục root

```
├── app                # Chứa phần core của source code
├── bootstrap          # Chứa file khởi tạo ứng dụng
│   ├── Cache          # Chứa các cache routes và services
│   └── app.php        # Nơi khởi tạo ứng dụng
├── config             # Chứa tất cả các tệp cấu hình của ứng dụng
├── database           # Chứa các file migration, factory & seed
├── public
│   ├── images         # Chứa các static images
│   └── index.php      # Entry point của ứng dụng
├── resources          # Chứa các file view, lang, js, css
├── routes
│   ├── api.php        # Định nghĩa các route API
│   ├── channels.php   # Định nghĩa các channel
│   ├── console.php    # Định nghĩa các command
│   └── web.php        # Định nghĩa các route web
├── storage            # Chứa các tệp log, template Blade biên dịch, session, bộ nhớ cache
│   ├── app
│   ├── framework
│   └── logs
├── tests              # Chứa các auto test sử dụng các thư viện `Pest` hoặc `PHPUnit`
└── vendor             # Nơi chứa các thư viện bên thứ 3
```

## Thư mục app

Nơi chứa phần core của project của bạn, mặc định thư mục có namespace là `App` và được tự động load bởi composer sử dụng PSR-4

Mặc định App sẽ chứa Http, Models và Providers, nhưng sẽ được thêm vào theo câu leenhgj Artisan mà người dùng sử dụng.

Cả thử mục Console và Http có thể được coi là các cung cấp của một API, chúng chỉ cung cấp các cách tương tác khác nhau với Api của bạn. Trong khi Console cung cấp các command line, Http cung cấp các route và
controller.

```
├── Boardcasting      # Chứa các file xử lý realtime được tạo ra bởi câu lệnh make:channel
├── Console           # Chứa các file command line được tạo ra bởi câu lệnh make:command
├── Events            # Chứa các file xử lý sự kiện được tạo ra bởi câu lệnh make:event hoặc event:generate
├── Exceptions        # Chứa các file xử lý ngoại lệ
├── Http              # Chứa các logic xử lý request và response
│   ├── Controllers
│   ├── Middleware
│   └── Kernel.php    # Chứa các middleware và route middleware
├── Jobs              # Chứa các file xử lý job được tạo ra bởi câu lệnh make:job
├── Listeners         # Chứa các file xử lý sự kiện được tạo ra bởi câu lệnh make:listener hoặc event:generate
├── Mail              # Chứa các file xử lý mail được tạo ra bởi câu lệnh make:mail
├── Models            # Chứa các file model
├── Notifications     # Chứa các file xử lý thông báo được tạo ra bởi câu lệnh make:notification
├── Policies          # Chứa các file xử lý phân quyền được tạo ra bởi câu lệnh make:policy
├── Providers         # Nơi khai báo các service provider
└── Rules             # Chứa các file xử lý validation được tạo ra bởi câu lệnh make:rule

```
