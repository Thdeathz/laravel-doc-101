# Security

## Nội dung

| Content                                   |
| :---------------------------------------- |
| [Authentication](#authentication)         |
| [Authorization](#authorization)           |
| [Email Verification](#email-verification) |
| [Encryption](#encryption)                 |
| [Hashing](#hashing)                       |
| [Password Reset](#password-reset)         |

## More info

### Authentication

`Authentication` là quá trình xác thực người dùng, đảm bảo họ có quyền truy cập vào hệ thống.

Mặc định laravel cung cấp các `guards` và `providers` để xác thực người dùng. Thông tin phiên đăng nhập được lưu trữ trong `session`. Và được config trong `config/auth.php`

---

### Authorization

`Authorization` là quá trình kiểm tra xem người dùng có quyền truy cập vào một tài nguyên hay không.

Mặc định laravel cung cấp nhiều cách để kiểm tra quyền người dùng như `Policies`, `Gates`, `middleware`,...

---

### Email Verification

Cung cấp các tính năng liên quan tới việc xác thực email đăng nhập của người dùng có hợp lệ hay không.

---

### Encryption

Laravel cung cấp service encryption mạnh mẽ thông qua `Illuminate\Support\Facades\Crypt` cung cấp các phương thức mã hõa theo chuẩn của OpenSSL với các phương thức mã hóa như AES-256 và AES-128.

Được cấu hình trong thư mục `config/app.php`. Mặc định trước khi khởi chạy ứng dụng, laravel yêu cầu tạo một `APP_KEY` để mã hóa dữ liệu. Có thể tạo thông qua câu lệnh:

```bash
php artisan key:generate
```

```php
class DigitalOceanTokenController extends Controller
{
  /**
   * Store a DigitalOcean API token for the user.
   */
  public function store(Request $request): RedirectResponse
  {
    $request->user()->fill([
      'token' => Crypt::encryptString($request->token),
    ])->save();

    return redirect('/secrets');
  }
}
```

---

### Hashing

Cung cấp các phương thức mã hóa mật khẩu của người dùng. Mặc định sử dụng `bcrypt` để mã hóa mật khẩu.

```php
$request->user()->fill([
  'password' => Hash::make($request->newPassword)
])->save();
```

---

### Password Reset

Cung cấp các service để triển khai chức năng reset mật khẩu cho người dùng.
