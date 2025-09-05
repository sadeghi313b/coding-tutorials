فرض کن یک مدل `User` داریم با فیلدهای زیر:
```php
id, first_name, last_name, email, password, created_at, updated_at
```
---
### ارسال مستقیم مدل‌ها به Vue بدون Resource
```php
// Controller
public function index()
{
    $users = User::all();  // تمام رکوردها
    return Inertia::render('Users/Index', [
        'users' => $users
    ]);
}
```
- JSON ارسال شده به Vue شبیه این است:
```json
[
    {
        "id": 1,
        "first_name": "Ali",
        "last_name": "Ahmadi",
        "email": "ali@example.com",
        "password": "$2y$10$xyz...",
        "created_at": "2025-09-01T10:00:00",
        "updated_at": "2025-09-01T10:00:00"
    }
]
```
**مشکل‌ها:**
1. فیلد `password` هم ارسال شده (حساس)
2. `first_name` و `last_name` جدا هستند و ممکن است در Vue بخواهیم با یک فیلد `full_name` داشته باشیم
3. فرمت تاریخ ممکن است نیاز به تغییر باشد (مثلاً فقط تاریخ بدون زمان)
---
### ۲️⃣ استفاده از Resource برای اصلاح خروجی
```php
// app/Http/Resources/UserResource.php
namespace App\Http\Resources;
use Illuminate\Http\Resources\Json\JsonResource;
class UserResource extends JsonResource
{
    public function toArray($request)
    {
        return [
            'id' => $this->id,
            'full_name' => $this->first_name . ' ' . $this->last_name,
            'email' => $this->email,
            'created_at' => $this->created_at->format('Y-m-d'), // فقط تاریخ
        ];
    }
}
```
```php
// Controller
public function index()
{
    $users = UserResource::collection(User::all());
    return Inertia::render('Users/Index', [
        'users' => $users
    ]);
}
```
- JSON ارسال شده اکنون:
```json
[
    {
        "id": 1,
        "full_name": "Ali Ahmadi",
        "email": "ali@example.com",
        "created_at": "2025-09-01"
    }
]
```
✅ مزایا:
- فیلد حساس `password` حذف شد
- نام کامل ساخته شد (`full_name`)
- فرمت تاریخ مناسب Vue شد
---
اگر بخواهی، می‌توانم یک **مثال مشابه برای ResourceCollection** هم بزنم که شامل **pagination و computed properties برای Vue** باشد تا استفاده حرفه‌ای‌تر مشخص شود.
می‌خوای این نمونه را هم نشان بدهم؟