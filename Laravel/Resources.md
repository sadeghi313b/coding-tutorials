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

اگر بخواهی، می‌توانم یک **مثال مشابه برای ResourceCollection** هم بزنم که شامل **pagination و computed properties برای Vue** باشد تا استفاده حرفه‌ای‌تر مشخص شود.
می‌خوای این نمونه را هم نشان بدهم؟


# فرمت دیتای ارسالی به فرانت
با سلاماینجا نکته‌ی اصلی همون **`UserResource::collection($paginator)`** هست.  
وقتی از Resource استفاده می‌کنی، لاراول ساختار JSON رو یک لایه‌ی اضافی می‌پیچه.


### تفاوت حالت عادی و Resource
1. اگر مستقیم `$paginator` رو بفرستی:
```php
return Inertia::render('Users/Index', [
    'users' => $paginator,
]);
```
خروجی سمت فرانت به شکل زیره:
```json
{
  "data": [ {..}, {..}, ... ],
  "current_page": 1,
  "per_page": 3,
  "total": 15,
  ...
}
```
2. اما وقتی می‌نویسی:
```php
return Inertia::render('Users/Index', [
    'users' => UserResource::collection($paginator),
]);
```
خروجی اینطوری میشه:
```json
{
  "data": {
    "data": [ {..}, {..}, ... ],
    "links": {..},
    "meta": {
      "current_page": 1,
      "per_page": 3,
      "total": 15,
      ...
    }
  }
}
```
یعنی متای صفحه‌بندی میره داخل **`meta`**، و داده‌ها همچنان داخل **`data`** هستن.
---
### راه‌حل در Vue
باید اینطوری بخونی:
```js
const rows = props.users.data; // این همون آرایه از یوزرهاست
const pagination = reactive({
  page: props.users.meta.current_page,
  rowsPerPage: props.users.meta.per_page,
  rowsNumber: props.users.meta.total,
});
```

### خلاصه
- اگر `$paginator` خام رو بفرستی → `props.users.current_page`
- اگر `UserResource::collection($paginator)` بفرستی → `props.users.meta.current_page`

می‌خوای من برایت یک `usePaginator` composable بنویسم که خودش تفاوت این دو حالت رو هندل کنه و همیشه `rows` و `pagination` استاندارد بده به Quasar؟
<q-pagination
                                                v-model="pagination.page"
                                                :max="users.meta.last_page"
                                                input
                                                size="sm"
                                                @update:model-value="onPaginationChange"
                                            />