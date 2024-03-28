# Testing

## Nội dung

| Content                             |
| :---------------------------------- |
| [Getting started](#getting-started) |
| [HTTP Test](#http-test)             |
| [Console Test](#console-test)       |
| [Browser Test](#browser-test)       |
| [Database](#database)               |
| [Mocking](#mocking)                 |

## More info

### Getting Started

Skeleton project của laravel đã được setup sẵn phù hợp cho việc testing. Laravel hỗ trợ viết test với `PHPUnit` và
`Pest` được cấu hình sẵn trong `phpunit.xml`.

Có thể tạo test với các câu lệnh:

```bash
php artisan make:test UserTest
```

Sau khi tạo test, có thể chạy test với câu lệnh:

```bash
php artisan test
```

---

### HTTP Test

Kiểm thử ứng dụng với HTTP request thông qua `PHPUnit`:

```php
<?php

namespace Tests\Feature;

use Tests\TestCase;

class ExampleTest extends TestCase
{
  /**
   * A basic test example.
   */
  public function test_the_application_returns_a_successful_response(): void
  {
    $response = $this->get('/');

    $response->assertStatus(200);
  }
}
```

---

### Console Test

Kiểm thử ứng dụng với console command thông qua `PHPUnit`:

```php
/**
 * Test a console command.
 */
public function test_console_command(): void
{
  $this->artisan('inspire')->assertExitCode(0);
}
```

---

### Browser Test

Kiểm thử ứng dụng với browser thông qua [Laravel Dusk](https://github.com/laravel/dusk):

---

### Database

Cho phép tương tác với database trong quá trình test thông qua `PHPUnit`:

```php
<?php

namespace Tests\Feature;

use Illuminate\Foundation\Testing\RefreshDatabase;
use Tests\TestCase;

class ExampleTest extends TestCase
{
  use RefreshDatabase;

  /**
   * A basic functional test example.
   */
  public function test_basic_example(): void
  {
    $response = $this->get('/');

    // ...
  }
}
```

---

### Mocking

Mocking giúp tạo ra các đối tượng giả mạo để kiểm thử:

```php
use App\Service;
use Mockery;
use Mockery\MockInterface;

public function test_something_can_be_mocked(): void
{
  $this->instance(
    Service::class,
    Mockery::mock(Service::class, function (MockInterface $mock) {
      $mock->shouldReceive('process')->once();
    })
  );
}
```
