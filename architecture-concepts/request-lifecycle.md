# Vòng đời một request

## Nội dung

| Content                                                   |
| :-------------------------------------------------------- |
| [Lifecycle Overview](#lifecycle-overview)                 |
| [Focus on Service Providers](#focus-on-service-providers) |

## Lifecycle Overview

### First Steps

Entry point của tất cả các req vào ứng dụng Laravel là `public/index.php`, được cấu hình bằng cách sử dụng các web server như Apache hoặc Nginx.

Trong `index.php` sẽ tải các extension bằng cách load `/vendor/autoload.php` file được tạo tự động bởi Composer, sau đó load cá thể hiện cần thiết cho các service quan trọng trong `/bootstrap/app.php`. Hành động đầu tiên của larvel là tạo ra một instance của ứng dụng [service container](./service-container.md)

---

### HTTP / Console Kernels

Tiếp đó request gửi tới sẽ được chuyển tếp sang HTTP kernel hoặc Console kernel, tùy thuộc vào loại request.

HTTP kenal khởi tọa một mảng các `bootstrappers` nó sẽ được chạy trước khi request được thực thi. `bootstrappers` sẽ cấu hình các error handling, logging, và các service providers.

Ngoài ra ta có thể thêm các `middleware` vào HTTP kernel, mỗi middleware sẽ được chạy trước khi request được xử lý, và có thể thay đổi request hoặc response.

---

### Service Providers

Một trong các actions quan trọng của kernal bootstrapping là tải các service providers. Các service này chị trách nghiệm khởi động tất các các thần phần cầ thiế của framework như database, queue, validation, và routing.

---

### Routing

Sau khi tất các các service providers đã được tải, request sẽ được chuyển tới router, nó sẽ xác định route phù hợp với request và thực thi controller.

---

### Finishing Up

Khi một response được trả về trong controller, nó sẽ được chạy qua các route's middleware cho phép chỉnh sửa lần cuối.

Cuối cùng HTTP kernel với phương thức `handle` sẽ trả về response object cho `handleRequest` của application instance, và nó sẽ gọi phương thức `send` để trả về response. Phương thức `send` sẽ gửi reponse content tới người dùng và kết thúc vòng đời của một request.

## Focus on Service Providers

Các service providers được khai báo và lưu trữ trong `app/Providers`.

Mặc định `AppServiceProvider` khá trống, và đây là nơi để thêm các ràng buộc, binding, và các service khác vào container

```php
namespace App\Providers;

use Illuminate\Support\ServiceProvider;

class AppServiceProvider extends ServiceProvider
{
    public function register()
    {
        //
    }

    public function boot()
    {
        //
    }
}
```
