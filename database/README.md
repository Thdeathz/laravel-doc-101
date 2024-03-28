# Databases

## Nội dung

| Content                             |
| :---------------------------------- |
| [Getting started](#getting-started) |
| [Query Builder](#query-builder)     |
| [Pagination](#pagination)           |
| [Migration](#migration)             |
| [Seeding](#seeding)                 |
| [Redis](#redis)                     |

## More info

### Getting Started

Laravel cho phép kết nối với nhiều loại database khác nhau cũng như cung cấp một số cách để làm việc với database như `Eloquent ORM`, `Query Builder`, `Raw SQL`,...

Việc cấu hình database được thực hiện trong `config/database.php`.

---

### Query Builder

`Query Builder` là một cách để tạo câu truy vấn SQL mà không cần viết trực tiếp câu lệnh SQL. Cho phép chia nhỏ câu truy vấn thành các phần nhỏ hơn để dễ dàng xây dựng và bảo trì.

```php
$users = DB::table('users')->get();
```

---

### Pagination

Cung cấp các phương thức phân trang dữ liệu trả về từ database.

```php
$users = DB::table('users')->paginate(15);
```

---

### Migration

Migration giúp tạo và quản lý cấu trúc database thông qua các file PHP. Mỗi file migration chứa các phương thức `up` và `down` để tạo và xóa bảng, chỉnh sửa cấu trúc database.

```bash
php artisan make:migration create_users_table
```

---

### Seeding

Seeding giúp tạo dữ liệu mẫu cho database thông qua các file PHP. Ứng dụng `Factory pattern` và thư viện `Faker` để tạo dữ liệu mẫu, cho phép tạo hành triệu dữ liệu mẫu một cách dễ dàng.

```php
use App\Models\User;

public function run(): void
{
  User::factory()
        ->count(50)
        ->hasPosts(1)
        ->create();
}
```

---

### Redis

Laravel cung cấp sẵn các phương thức để làm việc với caching database là `Redis` thông qua facade `Illuminate\Support\Facades\Redis`.

```php
public function show(string $id): View
{
  return view('user.profile', [
    'user' => Redis::get('user:profile:'.$id)
  ]);
}
```
