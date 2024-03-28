# Eloquent ORM

## Nội dung

| Content                                  |
| :--------------------------------------- |
| [Getting started](#getting-started)      |
| [Relationships](#relationships)          |
| [Collections](#collections)              |
| [Mutators & Casting](#mutators--casting) |
| [Api Resources](#api-resources)          |
| [Serialization](#serialization)          |
| [Factories](#factories)                  |

## More info

### Getting Started

Mặc định laravel hỗ trợ `Eloquent` là một ORM siêu mạnh giúp tương tác với database một cách dễ dàng và linh hoạt.

```php
use App\Models\User;

$users = User::all();
```

---

### Relationships

`Eloquent` hỗ trợ nhiều loại quan hệ giữa các model như `One To One`, `One To Many`, `Many To Many`, `Has Many Through`, etc,.. Giúp việc tương tác với các bảng có nhiều quan hệ trong database trở nên dễ dàng.

```php
namespace App\Models;

class User extends Model
{
  public function phone()
  {
    return $this->hasOne(Phone::class);
  }

  public function posts()
  {
    return $this->hasMany(Post::class);
  }

  public function roles()
  {
    return $this->belongsToMany(Role::class);
  }

  public function country()
  {
    return $this->hasOneThrough(Country::class, Address::class);
  }
}
```

---

### Collections

Tất cả các phương thức trong `Eloquent` trả về một collection, thay vì một mảng. Collection giúp xử lý dữ liệu một cách dễ dàng và linh hoạt.

```php
$names = User::all()->reject(function (User $user) {
  return $user->active === false;
})->map(function (User $user) {
  return $user->name;
});
```

---

### Mutators & Casting

Cho phép thay đổi giá trị của các thuộc tính trước khi lưu vào database hoặc sau khi lấy ra từ database thông qua `Mutator` và `Cast`.

```php
/**
 * Get the user's first name.
 */
protected function firstName(): Attribute
{
  return Attribute::make(
    get: fn (string $value) => ucfirst($value),
  );
}
```

---

### Api Resources

`Api Resources` giúp chuyển đổi dữ liệu của model thành dạng json một cách sạch sẽ và dễ dàng.

Có thể tạo một resource bằng câu lệnh:

```bash
php artisan make:resource UserResource
```

```php
/**
 * Transform the resource into an array.
 *
 * @return array<string, mixed>
 */
public function toArray(Request $request): array
{
  return [
    'id' => $this->id,
    'name' => $this->name,
    'email' => $this->email,
    'created_at' => $this->created_at,
    'updated_at' => $this->updated_at,
  ];
}
```

---

### Serialization

Cung cấp một số phương thức có sẵn giúp chuyến đổi dữ liệu từ modal sang dạng json một cách dễ dàng.

```php
return $response->json(User::all()->toArray());
```

---

### Factories

Ứng dụng `Factory pattern` giúp tạo các kho dữ liệu giả từ đó có thể tạo dummy data một cách dễ dàng.
