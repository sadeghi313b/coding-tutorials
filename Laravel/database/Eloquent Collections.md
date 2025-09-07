# جزوه تشریحی Eloquent Collections در Laravel
# BaseCollection vs EloquentCollection

| ویژگی    | Base Collection                        | Eloquent Collection                                |
| -------- | -------------------------------------- | -------------------------------------------------- |
| کلاس     | `Illuminate\Support\Collection`        | `Illuminate\Database\Eloquent\Collection`          |
| نوع داده | هر نوع داده (آرایه، اشیاء و غیره)      | فقط مدل‌های Eloquent                               |
| روش‌ها   | روش‌های عمومی (`map`، `filter` و غیره) | روش‌های عمومی + روش‌های اختصاصی (`load`، `append`) |
| روابط    | پشتیبانی نمی‌کند                       | پشتیبانی از eager loading و روابط                  |
| تبدیل    | خروجی پیش‌فرض بسیاری از روش‌ها         | برخی روش‌ها به base collection تبدیل می‌شوند       |
| کاربرد   | داده‌های عمومی و غیرمدلی               | مدل‌های دیتابیس و روابط                            |

## خلاصه کلی

در واقع Eloquent Collections در Laravel، کلاس `Illuminate\Database\Eloquent\Collection` است که از base collection (`Illuminate\Support\Collection`) ارث‌بری می‌کند. این کلاس ابزارهای قدرتمندی برای مدیریت مجموعه‌های مدل‌های Eloquent ارائه می‌دهد. علاوه بر روش‌های base collection مانند `map`، `filter` و `reject`، روش‌های اختصاصی مانند `load`، `fresh` و `append` برای کار با مدل‌ها و روابط آنها فراهم شده است. این مجموعه‌ها به‌عنوان iterator عمل می‌کنند و امکان زنجیره‌سازی عملیات را دارند.

### سینتکس کلی

- دریافت مجموعه: <span dir=ltr>$users = User::get();</span> یا از روابط (`$post->comments`).
- حلقه‌زدن: `foreach ($users as $user) { echo $user->name; }`
- زنجیره‌سازی: `$names = User::all()->reject(fn($user) => $user->active === false)->map(fn($user) => $user->name);`
- تبدیل به base collection: روش‌هایی مانند `pluck`، `collapse` یا `map` (اگر خروجی مدل Eloquent نباشد).
- روش‌های اختصاصی: `$users->load('comments');`، `$users->append('full_name');`

### جدول خلاصه روش‌ها

| method      | توضیح مختصر                            | سینتکس نمونه                                              |
| ----------- | -------------------------------------- | --------------------------------------------------------- |
| append      | افزودن attribute به خروجی مدل‌ها       | `$users->append('team');`                                 |
| contains    | بررسی وجود مدل در مجموعه               | `$users->contains(1);`                                    |
| diff        | مدل‌های غیرمشترک با مجموعه دیگر        | `$users->diff($otherUsers);`                              |
| except      | حذف مدل‌ها بر اساس کلید                | `$users->except([1,2]);`                                  |
| find        | پیدا کردن مدل با کلید                  | `$users->find(1);`                                        |
| findOrFail  | پیدا کردن یا پرتاب exception           | `$users->findOrFail(1);`                                  |
| fresh       | بارگذاری تازه مدل‌ها از دیتابیس        | `$users->fresh('comments');`                              |
| intersect   | مدل‌های مشترک با مجموعه دیگر           | `$users->intersect($otherUsers);`                         |
| load        | eager loading روابط                    | `$users->load('comments');`                               |
| loadMissing | eager loading روابط بارنشده            | `$users->loadMissing('comments');`                        |
| modelKeys   | کلیدهای اصلی مدل‌ها                    | `$users->modelKeys();`                                    |
| makeVisible | نمایش attributeهای پنهان               | `$users->makeVisible('address');`                         |
| makeHidden  | پنهان کردن attributeهای قابل مشاهده    | `$users->makeHidden('address');`                          |
| only        | انتخاب مدل‌ها با کلیدها                | `$users->only([1,2]);`                                    |
| partition   | تقسیم مجموعه به دو زیرمجموعه           | `$partition = $users->partition(fn($u) => $u->age > 18);` |
| setVisible  | override موقت attributeهای قابل مشاهده | `$users->setVisible(['id','name']);`                      |
| setHidden   | override موقت attributeهای پنهان       | `$users->setHidden(['email']);`                           |
| toQuery     | تبدیل به query builder                 | `$users->toQuery()->update(...);`                         |
| unique      | مدل‌های منحصربه‌فرد                    | `$users->unique();`                                       |

## مقدمه (Introduction)

اینجا Eloquent Collections برای مدیریت نتایج چندمدلی از کوئری‌های Eloquent (مانند `get()` یا روابط) استفاده می‌شود. این مجموعه‌ها از base collection ارث‌بری کرده و قابلیت‌های پیشرفته‌ای مانند زنجیره‌سازی عملیات، eager loading روابط و مدیریت attributes را فراهم می‌کنند.

### سینتکس کلی

- دریافت مجموعه: `$users = User::where('active', 1)->get();`
- حلقه‌زدن: `foreach ($users as $user) { echo $user->name; }`
- زنجیره‌سازی: `$names = User::all()->reject(fn($user) => $user->active === false)->map(fn($user) => $user->name);`
- تبدیل: روش‌هایی مانند `collapse`، `flatten` به base collection تبدیل می‌شوند.

### جدول ویژگی‌ها

| ویژگی    | توضیح                               |
| -------- | ----------------------------------- |
| ارث‌بری  | از `Illuminate\Support\Collection`  |
| iterator | قابل حلقه‌زدن مانند آرایه           |
| قدرت     | عملیات map/reduce با رابط intuitive |

### توضیحات و مثال

مجموعه Eloquent Collections امکان کار با مدل‌ها را به‌صورت روان و انعطاف‌پذیر فراهم می‌کند. برای مثال، فرض کنید می‌خواهید کاربران فعال را فیلتر کرده و نام‌هایشان را استخراج کنید:

```php ln=false
$names = User::all()->reject(function ($user) {
    return $user->active === false;
})->map(function ($user) {
    return $user->name;
});
```

این کد کاربران غیرفعال را حذف کرده و آرایه‌ای از نام‌ها تولید می‌کند.

## تبدیل مجموعه Eloquent (Eloquent Collection Conversion)

برخی روش‌ها مانند `collapse`، `flatten`، `flip`، `keys`، `pluck` و `zip` مجموعه را به base collection تبدیل می‌کنند. همچنین، اگر `map` خروجی بدون مدل Eloquent تولید کند، نتیجه یک base collection خواهد بود.

### سینتکس کلی

- روش‌های تبدیل‌کننده: `collapse()`، `flatten()`، `flip()`، `keys()`، `pluck()`، `zip()`
- تبدیل در `map`: `$users->map(fn($user) => $user->name); // base collection`

### توضیحات و مثال

فرض کنید مجموعه‌ای از کاربران دارید و می‌خواهید فقط ایمیل‌ها را استخراج کنید:

```php
$emails = User::all()->pluck('email'); // Illuminate\Support\Collection
```

این کد یک base collection از ایمیل‌ها برمی‌گرداند، نه Eloquent Collection.

## روش‌های موجود (Available Methods)

Eloquent Collections تمام روش‌های base collection را به ارث می‌برد و روش‌های اختصاصی برای مدیریت مدل‌ها اضافه می‌کند.

### سینتکس کلی

- فراخوانی: `$collection->method(...);`
- بازگشت: معمولاً `Illuminate\Database\Eloquent\Collection`، جز در مواردی مانند `modelKeys` که `Illuminate\Support\Collection` برمی‌گرداند.

## append
متد `append($attributes)` برای افزودن attributes (ویژگی‌ها یا accessors) به خروجی سریال‌سازی مدل‌ها (مانند JSON) استفاده می‌شود، بدون تغییر در دیتابیس. مثلا فرض کنید که فیلدهای firstName و lastName موجودند و ما یک attribute به نام fullName به مدل(نه دیتابیس) با append اضافه میکنیم که در هنگام خروجی سریال سازی به جیسون یا آرایه تبدیل میشود. این روش ستونی به جدول اضافه نمی‌کند، بلکه فقط داده‌های خروجی را غنی می‌کند.
### سینتکس کلی

`$users->append('attribute_name');` یا 
`$users->append(['attribute1', 'attribute2']);`

### توضیحات و مثال
```php ln=false
...
class User extends Model {
    public function getFullNameAttribute() {
        return $this->first_name . ' ' . $this->last_name;
    }
}

$users = User::all()->append('full_name');
echo $users->toJson();
```
output:
```json ln=false
[
    {"id": 1, "first_name": "Ali", "last_name": "Ahmadi", "full_name": "Ali Ahmadi"},
    ...
]
```
## contains
بررسی وجود مدل در مجموعه بر اساس کلید اصلی یا نمونه مدل.
`$users->contains(1);` یا 
`$users->contains(User::find(1));`

```php ln=false
$users = User::all();
if ($users->contains(1)) {
    echo "the user of id=1 is exist";
}
```
## diff
مدل‌هایی را برمی‌گرداند که در مجموعه دیگر وجود ندارند.
`$users->diff($otherUsers$)->get());`

```php ln=false
$users = User::all();
$otherUsers = User::whereIn('id', [1,2,3])->get();
$diff = $users->diff($otherUsers); // کاربرانی که در $otherUsers نیستند
	-or-
$users->diff(User::whereIn('id', [1,2,3])->get());
```

## except
حذف مدل‌ها بر اساس کلیدهای اصلی.
`$users->except([1,2,3]);`

```php ln=false
$users = User::all()->except([1,2]); //all users except user of id=1&2
```

## find
پیدا کردن مدل با کلید اصلی یا آرایه‌ای از کلیدها.
`$users->find(1);` یا `$users->find([1,2]);`

```php ln=false
$users = User::all();
$user = $users->find(1); // مدل با id=1
$subset = $users->find([1,2]); // مدل‌های با idهای 1 و 2
```

## findOrFail
پیدا کردن مدل یا پرتاب `ModelNotFoundException`. یعنی یک استثنا (exception) از نوع Illuminate\Database\Eloquent\ModelNotFoundException توسط فریم‌ورک ایجاد و به سمت سیستم پرتاب می‌شود. این مکانیزم در Laravel برای مدیریت خطاها در کوئری‌های دیتابیس استفاده می‌شود. 
اگر مدل پیدا نشود (یعنی هیچ رکوردی با کلید یا شرایط مشخص‌شده وجود نداشته باشد)، به‌جای بازگرداندن null یا یک نتیجه خالی، Laravel یک استثنای ModelNotFoundException تولید می‌کند. این استثنا به شما امکان می‌دهد خطا را مدیریت کنید (مثلاً با نمایش پیام خطا به کاربر یا هدایت به صفحه‌ای دیگر).
`$users->findOrFail(1);`

```php ln=false
try {
    $user = User::all()->findOrFail(999); // اگر پیدا نشود، exception پرتاب می‌شود
} catch (\Illuminate\Database\Eloquent\ModelNotFoundException $e) {
    echo "کاربر یافت نشد";
}
```

میتوان مدیریت خطا را بصورت global انجام داد:
```php ln=false title=app/Exceptions/Handler.php
namespace App\Exceptions;

use Illuminate\Database\Eloquent\ModelNotFoundException;
use Illuminate\Foundation\Exceptions\Handler as ExceptionHandler;

class Handler extends ExceptionHandler {
    public function render($request, $exception) {
        if ($exception instanceof ModelNotFoundException) {
            return response()->json(['error' => 'مدل یافت نشد'], 404);
        }
        return parent::render($request, $exception);
    }
}
```

## fresh
بارگذاری تازه مدل‌ها از دیتابیس با امکان eager loading. این روش زمانی کاربرد دارد که می‌خواهید داده‌های یک مجموعه مدل را از دیتابیس به‌روزرسانی کنید تا اطمینان حاصل شود که اطلاعات تازه و همگام با دیتابیس هستند. همچنین زمانیکه میخواهید روابط خاصی را به‌صورت eager loading بارگذاری کنید. این متد تمام مدل‌های موجود در مجموعه را با داده‌های فعلی دیتابیس جایگزین می‌کند.
- سناریو: فرض کنید یک کاربر اطلاعاتی را در دیتابیس به‌روزرسانی کرده، اما مجموعه Eloquent Collection شما هنوز داده‌های قدیمی را دارد.
- سناریو: شما یک مجموعه کاربران دارید و حالا نیاز دارید رابطه comments را برای همه آنها بارگذاری کنید.
- سناریو: داده‌های یک مدل در کش ذخیره شده، اما دیتابیس به‌روزرسانی شده است. در سیستم‌هایی که از کش استفاده می‌شود (مانند Redis یا Memcached)، ممکن است داده‌های مدل در حافظه قدیمی شده باشند.
- سناریو: مدیریت تراکنش‌های همزمان: در برنامه‌هایی با کاربران متعدد که داده‌ها را تغییر می‌دهند. مثلا یک ادمین کاربری را غیرفعال کرده، اما مجموعه شما هنوز او را فعال نشان می‌دهد.
- در هنگام توسعه یا دیباگ، ممکن است بخواهید مطمئن شوید که داده‌های مدل با دیتابیس همگام هستند، به‌ویژه اگر در حال تست تغییرات دیتابیس هستید.
- تضمین داده‌های به‌روز در APIها: در APIها که نیاز به نمایش داده‌های تازه دارید، به‌ویژه برای پاسخ‌های حساس به زمان.
`$users->fresh();` // بدون رابطه
`$users->fresh('comments');` // با یک رابطه
`$users->fresh(['comments', 'posts']);` // با چندین رابطه

```php ln=false
$users = User::all()->fresh('comments'); // مدل‌ها با روابط comments تازه‌سازی می‌شوند
```

### مقایسه
- متد **fresh()**: کل مدل‌ها را از دیتابیس بازخوانی می‌کند و می‌تواند روابط را eager load کند. داده‌های فعلی مجموعه جایگزین می‌شوند.
- متد **load()**: فقط روابط مشخص‌شده را بارگذاری می‌کند، بدون تغییر داده‌های اصلی مدل.
- متد **loadMissing()**: روابطی که هنوز بار نشده‌اند را بارگذاری می‌کند، بدون بازخوانی داده‌های مدل.
- متد refresh(): داده‌های مدل فعلی را به‌روزرسانی می‌کند و فقط روی مدل‌های تکی کار می‌کند، نه مجموعه‌ها.
## intersect
مدل‌های مشترک با مجموعه دیگر.
`$users->intersect(User::whereIn('id', [1,2,3])->get());`

```php ln=false
$users = User::all();
$otherUsers = User::whereIn('id', [1,2,3])->get();
$common = $users->intersect($otherUsers); // فقط کاربران با idهای 1، 2، 3
```

## load
متدی برای eager loading روابط.

`$users->load(['comments', 'posts']);` یا 
`$users->load(['posts' => fn($q) => $q->where('active', 1)]);`

```php ln=false
$users = User::all()->load('comments'); // بارگذاری رابطه comments برای همه کاربران
```

## loadMissing
متد برای eager loading روابطی که هنوز بار نشده‌اند.
`$users->loadMissing(['comments', 'posts']);`

```php ln=false
$users = User::with('posts')->get();
$users->loadMissing('comments'); // فقط comments بار می‌شود اگر قبلاً بار نشده باشد
```

## modelKeys
کلیدهای اصلی مدل‌ها را برمی‌گرداند. یعنی آرایه ای از primaryKeys را برمیگرداند.
`$users->modelKeys(); // [1,2,3]`

```php ln=false
$users = User::all();
$keys = $users->modelKeys(); // آرایه‌ای از idها
```

## makeVisible
نمایش attributes پنهان در خروجی. مفهوم attributes پنهان (hidden attributes) به ویژگی‌ها یا فیلدهای یک مدل Eloquent اشاره دارد که به‌صورت پیش‌فرض در خروجی سریال‌سازی مدل (مانند تبدیل به JSON یا آرایه با toArray() و toJson()) نمایش داده نمی‌شوند. این قابلیت برای مخفی کردن داده‌های حساس (مانند رمز عبور یا توکن‌ها) یا داده‌هایی که نیازی به نمایش در خروجی ندارند، استفاده می‌شود. این می تواند شامل ستونها، اتریبیوتهای محاسباتی accessors، یا برخی روابط relations شود.
`$users->makeVisible(['address', 'phone_number']);`

```php ln=false
$users = User::all()->makeVisible('address'); // address در خروجی JSON نمایش داده می‌شود
```
شما می‌توانید attributes پنهان را در مدل Eloquent با استفاده از آرایه $hidden تعریف کنید:
```php ln=false
namespace App\Models;
use Illuminate\Database\Eloquent\Model;
class User extends Model {
    protected $hidden = ['password', 'remember_token'];
}
```
وقتی مدل را به JSON تبدیل می‌کنید، attributes موجود در $hidden در خروجی ظاهر نمی‌شوند. کاربرد: حفاظت از داده‌های حساس، کاهش حجم خروجی، جلوگیری از نمایش داده‌هایی که برای کاربران نهایی یا API مصرف‌کننده لازم نیست.
## makeHidden
پنهان کردن attributes قابل مشاهده.
`$users->makeHidden(['address', 'phone_number']);`

```php ln=false
$users = User::all()->makeHidden('email'); // email از خروجی JSON حذف می‌شود
```

## only
انتخاب مدل‌ها با کلیدهای اصلی خاص.
`$users->only([1,2,3]);`

```php ln=false
$users = User::all()->only([1,2]); // فقط کاربران با idهای 1 و 2
```
متد only همیشه یک Eloquent Collection برمی‌گرداند، حتی اگر فقط یک کلید مشخص شده باشد. اما متد find  می‌تواند یک مدل تکی را برگرداند، بسته به اینکه ورودی یک کلید تکی یا آرایه‌ای از کلیدها باشد.
در آرگومان ورودی: متد only نمی‌تواند نمونه مدل را به‌عنوان ورودی قبول کند اما متد find میتواند نمونه مدل هم قبول کند.
## partition
تقسیم مجموعه به دو زیرمجموعه بر اساس شرط.
`$partition = $users->partition(fn($user) => $user->age > 18);`

| output        | نوع                                                   |
| ------------- | ----------------------------------------------------- |
| $partition    | `Illuminate\Support\Collection`                       |
| $partition[0] | `Illuminate\Database\Eloquent\Collection` (تطبیق شرط) |
| $partition[1] | `Illuminate\Database\Eloquent\Collection` (عدم تطبیق) |
```php ln=false
$users = User::all();
[$adults, $minors] = $users->partition(fn($user) => $user->age > 18);
```

## setVisible
متد برای override موقت attributes قابل مشاهده.
`$users->setVisible(['id', 'name']);`

```php ln=false
$users = User::all()->setVisible(['id', 'name']); 
// فقط id و name در خروجی
```
## setHidden
متد برای override موقت attributes پنهان.
`$users->setHidden(['email', 'password']);`

```php ln=false
$users = User::all()->setHidden(['email']); // email از خروجی حذف می‌شود
```
## toQuery
تبدیل به query builder با `whereIn`.
`$users->toQuery()->update(['status' => 'Administrator']);`

فرض کنید مجموعه‌ای از کاربران با status='VIP' دارید و می‌خواهید وضعیت آنها را به Administrator تغییر دهید:
```php ln=false
$users = User::where('status', 'VIP')->get();
$users->toQuery()->update(['status' => 'Admin']);
```
متد toQuery() در سناریوهایی مفید است که می‌خواهید عملیات‌های دیتابیسی را روی مجموعه‌ای از مدل‌ها انجام دهید. بدون toQuery()، ممکن است بخواهید روی تک‌تک مدل‌ها حلقه بزنید، که می‌تواند ناکارآمد باشد. 
- عملکرد: toQuery() بهینه‌تر از حلقه زدن روی مدل‌ها است، زیرا یک کوئری واحد با whereIn اجرا می‌کند. 
- خروجی: همیشه یک query builder برمی‌گرداند، بنابراین باید متدهای query builder (مانند get()، update()، delete()) را به آن زنجیره کنید. 
- محدودیت: فقط روی کلیدهای اصلی (id یا کلید اصلی تعریف‌شده در مدل) کار می‌کند.
## unique
مدل‌های منحصربه‌فرد بر اساس کلید اصلی.
`$users->unique();`

```php ln=false
$users = User::all()->unique(); // حذف مدل‌های تکراری
```

## مجموعه‌های سفارشی (Custom Collections)

ایجاد مجموعه‌های سفارشی برای مدل‌ها.

### سینتکس کلی

- با attribute: `#[CollectedBy(UserCollection::class)]`
- با روش: `public function newCollection(array $models = []): Collection { return new UserCollection($models); }`
- پشتیبانی eager loading: `$collection->withRelationshipAutoloading();`

### جدول

|روش|کاربرد|
|---|---|
|CollectedBy attribute|برای مدل خاص|
|newCollection method|override برای مدل یا base model|
|کاربرد کلی|برای همه مدل‌ها در base model|

### توضیحات و مثال

ایجاد یک مجموعه سفارشی:

```php
namespace App\Support;

use Illuminate\Database\Eloquent\Collection;

class UserCollection extends Collection {
    public function active() {
        return $this->filter(fn($user) => $user->active);
    }
}

namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use App\Support\UserCollection;

class User extends Model {
    public function newCollection(array $models = []): Collection {
        return new UserCollection($models);
    }
}

$users = User::all()->active(); // فقط کاربران فعال
```