# Service container

## Nội dung

| Content                   |
| :------------------------ |
| [Giới thiệu](#giới-thiệu) |
| [Binding](#binding)       |

### Giới thiệu

Đây là một công cụ Laravel cung cấp để quản lý các class dependency và tối ưu việc sử dụng dependency injection.

## Zero configuration resolution

Nếu một class không sử dụng bất ky dependencies nào mà chỉ phụ thuộc vào chính nó, bạn có thể chỉ cần truy cập nó từ service container:

```php
<?php

class Service
{
    // ...
}

Route::get('/', function (Service $service) {
    die($service::class);
});
```

---

### When Utilize the container

Nhờ vòa zero configuration resolution, bạn có thể truy cập một class từ service container mà không cần phải cấu hình bất kỳ điều gì.

```php
use Illuminate\Http\Request;

Route::get('/', function (Request $request) {
    // ...
});
```

## Binding

### Simple binding

Hầu hết các service container bindings sẽ được khai báo trong [service provider](./service-provider.md)

Với service provider, ta có thể truy cập tới bất cứ container nào với `$this->app`. Ta có thể đăng ký một container với phương thức `bind()`, nhận vào interface và class implementation.

```php
use App\Repositories\UserRepository;
use App\Repositories\UserRepositoryInterface;

$this->app->bind(UserRepositoryInterface::class, UserRepository::class);
```

Ngoài việc tương tác với container với service provider, ta có thể tương tác với container với `App` [facade](./facades.md)

```php
use App\Services\Transistor;
use Illuminate\Contracts\Foundation\Application;
use Illuminate\Support\Facades\App;

App::bind(Transistor::class, function (Application $app) {
  // ...
});
```

Có thể sử dụng `bindIf` để chắc chắn rằng chỉ đăng ký một container binding nếu nó chưa tồn tại.

```php
$this->app->bindIf(UserRepositoryInterface::class, UserRepository::class);
```

---

### Binding a singleton

Phương thức `singleton()` khác với bind phương thức này báo với laravel rằng khi khởi tạo class này chỉ cần khởi tạo một lần và sử dụng lại cho các lần sau.

```php
use App\Services\Transistor;
use App\Services\PodcastParser;
use Illuminate\Contracts\Foundation\Application;

$this->app->singleton(Transistor::class, function (Application $app) {
    return new Transistor($app->make(PodcastParser::class));
});
```

Cũng có thể sử dụng `singletonIf` để chắc chắn rằng chỉ đăng ký một container binding nếu nó chưa tồn tại.

---

### Binding Scoped Singletons

Khá tương đồng với [singleton](#binding-a-singleton), nhưng với `scoped()` các instances được đăng ký sẽ flushed khi mà laravel app bắt đầu một lifecycle mới.
