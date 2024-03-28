# Cấu hình

## Nội dung

| Content                                               |
| :---------------------------------------------------- |
| [Giới thiệu](#giới-thiệu)                             |
| [Cấu hình biến môi trường](#cấu-hình-biến-môi-trường) |
| [Truy cập biến config](#truy-cập-các-biến-config)     |
| [Caching cấu hình](#configuration-caching)            |
| [Publish cấu hình](#configuration-publishing)         |
| [Chế độ debug](#debug-mode)                           |
| [Chế độ bảo trì](#maintenance-mode)                   |

## Giới thiệu

Tất cả cấu hình được lưu trong thư mục `config`,
các file config này cho phép cấu hình các thông tin như database, mail server address, time zone, etc,.. hầu hết được đọc từ các biến môi trường từ file `.env`

---

#### Câu lệnh `about`

Hiển thị các cài đặt trong thư muc `config`:

```bash
php artisan about
```

Hiển thị config trong 1 file cụ thể:

```bash
php artisan config:show [file_name]
```

## Cấu hình biến môi trường

Có thể import linh hoạt các config dựa theo môi trường app đang chạy.

Laravel sử dụng thư viện [phpdotenv](https://github.com/vlucas/phpdotenv) để triển khai biến mối trường.

Mặc định ở thư mục root có chứa file `.env.example`, trong quá trình cài đặt sẽ tự động tại ra file `.env` từ file mẫu trên. Những cài đặt này sẽ được đọc trong file [config](#giới-thiệu).

---

#### Bảo mật cho file môi trường

File `.env` không nên được push lên cùng với source code.

Laravel hỗ trợ [encrypt](#mã-hóa-file-môi-trường) file `.env để có thể push cũng source code một cách an toàn hơn

---

#### Thêm file môi trường

Mặc định laravel sẽ load file `.env` khi load ứng dụng.

Nhưng có thể ghi đè file muốn load bằng câu lệnh:

```bash
php artisan server --env=local
```

Khi đó laravel sẽ load file `.env.local`

---

### Biến của file môi trường

| .env Value | env() Value  |
| :--------- | :----------- |
| true       | (bool) true  |
| (true)     | (bool) true  |
| false      | (bool) false |
| (false)    | (bool) false |
| empty      | (string) ''  |
| (empty)    | (string) ''  |
| null       | (null) null  |
| (null)     | (null) null  |

---

### Sử dụng biến môi trường

Trong PHP sử dụng biến super global `$_ENV`

Trong laravel sử dụng phương thức `env()`:

```php
env('VARIABLE_NAME', default_value)
```

---

### Kiểm tra môi trường đang chạy

Có thể được xác định bằng biến `APP_ENV` trong file `.env`

Có thể truy cập biến này bằng phương thức `envrionment()`:

```php
use Illuminate\Support\Facades\App;

$environment = App::environment();
```

### Mã hóa file môi trường

Nên mã hóa file `.env` trước khi push lên source code.

---

#### Mã hóa

Sử dụng câu lệnh:

```bash
php artisan env:encrypt
```

Câu lệnh sẽ mã hóa file `.env` và lưu vào trong file `.env.encrypted`

Có thể chọn key mã hóa bằng cờ `--key`:

```bash
php artisan env:encrypt --key=very-secret-key
```

---

### Giải mã

```bash
php artisan env:decrypt --key=very-secret-key
```

## Truy cập các biến config

Có thể truy cập các biến trong `.env` được đọc trong các file config thông qua phương thức get của `Config` hoặc phương thức `config()`

```php
use Illuminate\Support\Facades\Config;

$value = Config::get('app.timezone');

$value = config('app.timezone');

// Retrieve a default value if the configuration value does not exist...
$value = config('app.timezone', 'Asia/Seoul');
```

## Configuration Caching

Để tăng tốc thì ta có thể cache các cấu hình đã được load từ file `.env`

```bash
php artisan config:cache
```

Clear caching

```bash
php artisan config:clear
```

## Configuration Publishing

Hầu hết các config đều được publish trong app nhưng có thể làm việc này một cách thủ công hoặc publish những config mà tự tạo như `cors.php` hoặc `view.php`

```bash
php artisan config:publish

php artisan config:publish --all
```

## Debug mode

Option `debug` trong `config/app.php` quy định lượng thông tin lỗi có thể hiện thị cho người dùng. Mặc định thì nó sẽ được set trong `APP_DEBUG` trong file `.env`

> Ở local thì nên luôn để `APP_DEBUG` là `true`. Nhưng ở production mode thì luôn để `APP_DEBUG` là `false`

## Maintenance Mode

Chế độ maintenace cho phép "tắt" tạm thời ứng dụng của bạn với end user để có thể thực hiện các cập nhật.

Bản chất là thể hiện `HttpException` sẽ trả về một lỗi với status code là 503 và render ra trang lỗi

Để bất chế độ này gọi câu lệnh:

```bash
php artisan down
```

Một số cờ nên thêm:

- `refresh`: Khi muốn bảo trình duyệt tự động refresh page sau bao nhiêu lâu, được trả về trong trường `Refresh` của HTTP header
- `retry`: Được trả về trong trường `Retry-After`của HTTP header, báo với trình duyệt hảy chỉ thử lại sau bao lâu

```bash
php artisan down --refresh=15 --retry=60
```

---

### Bypasing Maintenance Mode

Có thể cung cấp một secret key để bỏ qua chế độ maintance bằng câu lệnh:

```bash
php artisan down --secret="very-secret-key"
```

Khi đó có thể bypass chế độ này bằng các truy cập đường dẫn
`https://example.com/very-secret-key`

Có thể bảo laravel gen hộ secret token bằng câu lệnh:

```bash
php artisan down --with-secret
```

---

### Pre-Rendering the Maintenance Mode View

Có thể chỉ định blade view cho trang lỗi bằng câu lệnh:

```bash
php artisan down --render="errors::503"

```

---

### Disabling Maintenance Mode

Để tắt cái chế độ maintance sử dụng câu lệnh:

```bash
php artisan up
```
