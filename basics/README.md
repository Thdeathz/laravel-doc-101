# The basics

## Nội dung

| Content                                       |
| :-------------------------------------------- |
| [Routing](#routing)                           |
| [Middleware](#middleware)                     |
| [CSRF Protection](#csrf-protection)           |
| [Controller](#controller)                     |
| [Requests](#requests)                         |
| [Response](#response)                         |
| [View](#view)                                 |
| [Blade templates](#blade-templates)           |
| [Asset Bundling (Vite)](#asset-bundling-vite) |
| [URL Generation](#url-generation)             |
| [Session](#session)                           |
| [Validation](#validation)                     |
| [Error Handling](#error-handling)             |
| [Logging](#logging)                           |

## More info

### Routing

Nới nhận các request từ client và tìm route phù hợp để xử lý request đó.

Nhiệm vụ chính là tìm route đã khai báo ở và so sánh với request đến từ client.

---

### Middleware

Middleware là một hàm có thể truy cập vào request object (req), response object (res) để inspecting hoặc filtering HTTP request/response. Ứng dụng điển hình cho middleware là để triển kha authentication/authorization,...

Các core middleware của Laravel hoặc các custom middleware được tạo ra sẽ được thêm vào trong file `app/Http/Middleware` và được khai báo trong `$middlewareGroups` ở `app/Http/Kernel.php`.

```php
protected $middlewareGroups = [
  'api' => [
    // \Laravel\Sanctum\Http\Middleware\EnsureFrontendRequestsAreStateful::class,
    \Illuminate\Routing\Middleware\ThrottleRequests::class.':api',
    \Illuminate\Routing\Middleware\SubstituteBindings::class,
  ],
];
```

---

### CSRF Protection

`CSRF` - (Cross-Site Request Forgery) là một dạng tấn công giả mạo yêu cầu thông qua user đã được xác thực.

Laravel triển khai cơ chế để bảo vệ người dùng thông qua `CSRF token`.

Mỗi phiên hoạt động của người dùng sẽ được cấp cho 1 `CSRF token` và được tự động quản lý bởi framework. Mục đích của token là xác định xem thằng người dùng đã đăng nhập có thực sự là người gửi request hay không.

Khi tạo một form, bạn cần thêm một thẻ `@csrf` vào form để Laravel tự động thêm `CSRF token` vào form.

```php
<form method="DELETE" action="/delete-database">
  @csrf
  ...
</form>
```

---

### Controller

Là một thành phần trong kiến trúc MVC nơi mà nhận request từ client, xử lý và trả về response.

---

### Requests

Laravel `Illuminate\Http\Request` là một class OOP cho phép tương tác với HTTP request được gửi từ client.

---

### Response

Laravel `Illuminate\Http\Response` là một class OOP cho phép tạo ra HTTP response để trả về cho client.

---

### View

Được lưu trong `resources/views` phân tách với code logic chứa mã nguồn html để hiển thị cho người dùng.

---

### Blade templates

Blade là một template engine được Laravel cung cấp giúp tạo ra các view file chứa code PHP và HTML. Có đuôi mở rộng là `.blade.php` và được lưu trong thư mục `resources/views`.

---

### Asset Bundling (Vite)

Cho phép tương tác với `Vite` (build tool frontend) để bundle các file assets như CSS, JS, images, fonts,... và sử dụng trong ứng dụng Laravel.

---

### URL Generation

Tool cho phép tạo các URL chuẩn cho ứng dụng Laravel.

```php
$post = App\Models\Post::find(1);

echo url("/posts/{$post->id}");

// http://example.com/posts/1
```

---

### Session

Cung cấp các khuôn mẫu để tương tác với session trong laravel.

Được cấu hình trong `config/session.php`

Laravel cung cấp nhiều Provider để lưu trữ session như `file`, `cookie`, `database`, `memcached`, `redis`, `dynamodb`,... Mặc định sử dụng `file` để lưu trữ session và được lưu trong `storage/framework/sessions`.

```php
/*
|--------------------------------------------------------------------------
| Default Session Driver
|--------------------------------------------------------------------------
|
| This option controls the default session "driver" that will be used on
| requests. By default, we will use the lightweight native driver but
| you may specify any of the other wonderful drivers provided here.
|
| Supported: "file", "cookie", "database", "apc",
|            "memcached", "redis", "dynamodb", "array"
|
*/

'driver' => env('SESSION_DRIVER', 'file'),
```

---

### Validation

Laravel cung cấp nhiều cách để validate dữ liệu như `Form Request Validation`, `Validate method`,... mặc định trong Controller sử dụng class trait `ValidatesRequests` để validate dữ liệu.

```php
public function store(Request $request)
{
  $validatedData = $request->validate([
    'title' => 'required|unique:posts|max:255',
    'body' => 'required',
  ]);
}
```

---

### Error Handling

Laravel cung cấp các cách để xử lý lỗi như `HTTP Exceptions`, `Error Handling`, `Logging`,...

`$exceptions` object được cung cấp bới `withExceptions` là một thể hiện của `Illuminate\Foundation\Configuration\Exception` và được sử dụng để xử lý các exception trong ứng dụng.

```php
use Illuminate\Foundation\Exceptions\Handler as ExceptionHandler;

public function report(Exception $exception)
{
  if ($exception instanceof CustomException) {
    // Handle the exception
  }

  return parent::report($exception);
}
```

---

### Logging

Mặc định Laravel sử dụng `Monolog` để ghi log và được cấu hình trong `config/logging.php`.

```php
'channels' => [
  'stack' => [
    'driver' => 'stack',
    'channels' => ['single'],
  ],
  'single' => [
    'driver' => 'single',
    'path' => storage_path('logs/laravel.log'),
    'level' => 'debug',
  ],
],
```
