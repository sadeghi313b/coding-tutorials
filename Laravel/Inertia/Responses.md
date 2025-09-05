## Creating responses
ساختن یک **Inertia response** ساده است. برای این کار کافی است در **کنترلر** یا **Route** لاراول، متد `Inertia::render()` را فراخوانی کنید.
این متد دو چیز دریافت می‌کند:
```php
Inertia::render('Page/ComponentName', [
    'prop1' => $value1,
    'prop2' => $value2,
    // ...
]);
```
1. مسیر 'Page/ComponentName': نام کامپوننت صفحه در پروژه جاوااسکریپت است.
2. آرایه دوم: داده‌ها یا props که صفحه دریافت می‌کند.
### مثال در Laravel
```php
// This is a controller method returning an Inertia response
use Inertia\Inertia;
class EventsController extends Controller
{
    public function show(Event $event)
    {
        return Inertia::render('Event/Show', [
            'event' => $event->only(
              'id',
              'title',
              'start_date',
              'description'
            ),
        ]);
        // Alternatively, you can use the inertia() helper...
        return inertia('Event/Show', [
            'event' => $event->only(
              'id',
              'title',
              'start_date',
              'description'
            ),
        ]);
    }
}
```
- متد `only()` باعث می‌شود تنها داده‌های لازم برای صفحه ارسال شوند.
- در این مثال، صفحه مقصد `Event/Show` است که معمولاً در مسیر `resources/js/Pages/Event/Show.(js|vue|svelte)` قرار دارد.
### نکات مهم
- برای سرعت بارگذاری بهتر، تنها داده‌های **حداقلی لازم** برای صفحه را برگردانید.
- همه داده‌هایی که از کنترلر ارسال می‌شوند، در سمت کلاینت قابل مشاهده هستند؛ بنابراین **اطلاعات حساس را از پاسخ حذف کنید**.
## Properties
### توضیح
برای انتقال داده از سرور به کامپوننت‌های صفحه، می‌توان از **Properties (Props)** استفاده کرد.  
می‌توان انواع مختلفی از مقادیر را به عنوان prop ارسال کرد، از جمله:
- انواع اولیه (Primitive types)
- آرایه‌ها و اشیاء (Arrays & Objects)
- انواع خاص لاراول که به صورت خودکار حل می‌شوند (مثل Eloquent models، Collections و API Resources)
### مثال در Laravel
```php
// This is an example of an Inertia response in a Laravel controller
use App\Models\User;
use Illuminate\Http\Resources\Json\JsonResource;
use Inertia\Inertia;
Inertia::render('Dashboard', [
    // مقادیر اولیه
    'title' => 'Dashboard',
    'count' => 42,
    'active' => true,
    // آرایه‌ها و اشیاء
    'settings' => ['theme' => 'dark', 'notifications' => true],
    // اشیاء Arrayable (Models، Collections و غیره)
    'user' => auth()->user(), // مدل Eloquent
    'users' => User::all(), // Collection Eloquent
    // API Resources
    'profile' => new UserResource(auth()->user()),
    // اشیاء Responsable
    'data' => new JsonResponse(['key' => 'value']),
    // Closures
    'timestamp' => fn() => now()->timestamp,
]);
```
### توضیح کد
- اشیاء **Arrayable** مانند مدل‌ها و Collections به صورت خودکار با استفاده از متد `toArray()` به آرایه تبدیل می‌شوند.
- اشیاء **Responsable** مانند API Resources و JSON Responses از طریق متد `toResponse()` حل می‌شوند و داده نهایی صفحه را تشکیل می‌دهند.
- با این روش می‌توان انواع داده‌های پیچیده را به صفحات Vue/React/Svelte بدون نیاز به تبدیل دستی ارسال کرد.
### نکته
استفاده از Props باعث می‌شود داده‌ها **به صورت مستقیم و امن** از سرور به کامپوننت‌ها منتقل شوند و مدیریت داده در صفحات بسیار ساده‌تر شود.
## ProvidesInertiaProperty interface
### توضیح
گاهی لازم است هنگام ارسال **props** به کامپوننت‌ها، از کلاس‌های سفارشی استفاده کنید که بتوانند خودشان را به **فرمت مناسب داده برای Inertia** تبدیل کنند.
- در حالی که **Laravel Arrayable** فقط اشیاء را به آرایه تبدیل می‌کند،
- اما **ProvidesInertiaProperty** امکان تبدیل‌های پیچیده و وابسته به context را فراهم می‌کند.
این رابط (interface) نیاز به متد `toInertiaProperty` دارد که یک **PropertyContext** دریافت می‌کند. این context شامل موارد زیر است:
- کلید پراپ: `$context->key`
- همه پراپ‌های صفحه: `$context->props`
- شیء Request جاری: `$context->request`
### مثال ساده
```php
// This is a custom class implementing ProvidesInertiaProperty
use Inertia\PropertyContext;
use Inertia\ProvidesInertiaProperty;
class UserAvatar implements ProvidesInertiaProperty
{
    public function __construct(protected User $user, protected int $size = 64) {}
    public function toInertiaProperty(PropertyContext $context): mixed
    {
        return $this->user->avatar
            ? Storage::url($this->user->avatar)
            : "https://ui-avatars.com/api/?name={$this->user->name}&size={$this->size}";
    }
}
```
### استفاده در کنترلر
```php
// This is an example of an Inertia response using the custom prop
Inertia::render('Profile', [
    'user' => $user,
    'avatar' => new UserAvatar($user, 128),
]);
```
- این کلاس به عنوان مقدار یک prop استفاده می‌شود.
- `toInertiaProperty` هنگام رندر صفحه فراخوانی می‌شود تا مقدار نهایی prop را تولید کند.
### مثال پیشرفته: ادغام با داده‌های shared
```php
// This is a custom class merging with shared data
use Inertia\Inertia;
use Inertia\PropertyContext;
use Inertia\ProvidesInertiaProperty;
class MergeWithShared implements ProvidesInertiaProperty
{
    public function __construct(protected array $items = []) {}
    public function toInertiaProperty(PropertyContext $context): mixed
    {
        // دسترسی به shared data با کلید پراپ
        $shared = Inertia::getShared($context->key, []);
        // ادغام با داده‌های جدید
        return array_merge($shared, $this->items);
    }
}
// اشتراک‌گذاری داده‌ها
Inertia::share('notifications', ['Welcome back!']);
// استفاده در رندر صفحه
return Inertia::render('Dashboard', [
    'notifications' => new MergeWithShared(['New message received']),
    // نتیجه نهایی: ['Welcome back!', 'New message received']
]);
```
### نکته
و - **PropertyContext** اجازه می‌دهد اطلاعات صفحه و کلید پراپ را داشته باشید، بنابراین می‌توان الگوهای پیشرفته مثل ادغام با داده‌های shared یا پردازش دینامیک داده‌ها را پیاده کرد.
- این روش بسیار مناسب است برای زمانی که پراپ‌ها نیاز به **منطق خاص یا تبدیل وابسته به context** دارند.
## ProvidesInertiaProperties interface
### توضیح
گاهی لازم است **props مرتبط** را گروه‌بندی کنیم تا بتوان آنها را در صفحات مختلف دوباره استفاده کرد. برای این کار می‌توان از رابط **ProvidesInertiaProperties** استفاده کرد.
این رابط نیاز به متدی به نام `toInertiaProperties` دارد که یک **RenderContext** دریافت می‌کند. این context شامل موارد زیر است:
- نام کامپوننت: `$context->component`
- شیء Request جاری: `$context->request`
متد `toInertiaProperties` باید **یک آرایه کلید-مقدار** برگرداند که به عنوان props صفحه استفاده می‌شوند.
---
### مثال
```php
// This is a custom class implementing ProvidesInertiaProperties
use App\Models\User;
use Illuminate\Container\Attributes\CurrentUser;
use Inertia\RenderContext;
use Inertia\ProvidesInertiaProperties;
class UserPermissions implements ProvidesInertiaProperties
{
    public function __construct(#[CurrentUser] protected User $user) {}
    public function toInertiaProperties(RenderContext $context): array
    {
        return [
            'canEdit' => $this->user->can('edit'),
            'canDelete' => $this->user->can('delete'),
            'canPublish' => $this->user->can('publish'),
            'isAdmin' => $this->user->hasRole('admin'),
        ];
    }
}
```
---
### استفاده در کنترلر
```php
// This is an example of using a ProvidesInertiaProperties class in a controller
public function index(UserPermissions $permissions)
{
    return Inertia::render('UserProfile', $permissions);
    // or...
    return Inertia::render('UserProfile')->with($permissions);
}
```
- می‌توان کلاس‌های prop را مستقیماً به متد `render()` یا `with()` پاس داد.
- این روش باعث می‌شود منطق مربوط به props در یک کلاس جداگانه مدیریت شود و قابلیت استفاده مجدد داشته باشد.
---
### ترکیب چند کلاس prop با سایر props
```php
public function index(UserPermissions $permissions)
{
    return Inertia::render('UserProfile', [
        'user' => auth()->user(),
        $permissions,
    ]);
    // or using method chaining...
    return Inertia::render('UserProfile')
        ->with('user', auth()->user())
        ->with($permissions);
}
```
- می‌توان چند کلاس prop را با دیگر داده‌ها در یک آرایه یا با روش chaining استفاده کرد.
- این کار باعث انعطاف بیشتر و مدیریت مرتب props در صفحات پیچیده می‌شود.
## Root template data
### توضیح
گاهی لازم است که داده‌های props صفحه در **Root Blade Template** برنامه در دسترس باشند. این مورد مخصوصاً زمانی مفید است که بخواهید متا تگ‌ها را اضافه کنید، مانند:
- meta description
- Twitter card meta tags
- Facebook Open Graph meta tags
داده‌های صفحه از طریق متغیر `$page` در Blade قابل دسترسی هستند.
### مثال دسترسی به prop در Blade
```blade
<meta name="twitter:title" content="{{ $page['props']['event']->title }}">
```
- `$page['props']` شامل همه props ارسال شده به صفحه جاوااسکریپت است.
- می‌توان به راحتی از آنها برای تولید تگ‌های متا استفاده کرد.
---
گاهی ممکن است بخواهید داده‌هایی را به Root Blade Template بدهید که **به صفحه جاوااسکریپت منتقل نشوند**. برای این کار از متد `withViewData` استفاده می‌کنیم.
### مثال استفاده از withViewData
```php
// This is an example of passing data to the root Blade template only
return Inertia::render('Event', ['event' => $event])
    ->withViewData(['meta' => $event->meta]);
```
- داده‌های تعریف‌شده با `withViewData` مشابه یک **متغیر معمولی Blade** در دسترس هستند.
### مثال دسترسی در Blade
```blade
<meta name="description" content="{{ $meta }}">
```
- در اینجا داده‌ها فقط برای Blade هستند و به کامپوننت صفحه ارسال نمی‌شوند.
- این روش برای اطلاعاتی که مختص SEO یا تگ‌های HTML هستند مناسب است و به جاوااسکریپت صفحه منتقل نمی‌شوند.
### توضیح دقیق‌تر:
1. **Blade**: قالب اصلی HTML برنامه شما است که رندر سمت سرور انجام می‌دهد. در واقع، فایل‌های Blade مانند `resources/views/app.blade.php` یا سایر Blade templates، root HTML برنامه شما را تولید می‌کنند.
2. **Vue/React/Svelte (صفحات Inertia)**: این کامپوننت‌ها داخل Blade template اصلی رندر می‌شوند و وظیفه مدیریت رابط کاربری سمت کلاینت (SPA) را دارند.
3. **ارتباط Blade و صفحات Inertia**:
    - Inertia یک **root Blade template** دارد که معمولاً شامل `<div id="app" data-page="{{ json_encode($page) }}"></div>` است.
    - تمام صفحات Vue/React/Svelte در همین div mount می‌شوند.
    - متغیر `$page` در Blade در دسترس است و شامل تمام props صفحات Inertia است.
4. **withViewData**: وقتی داده‌ای را با این متد ارسال می‌کنید، آن داده **فقط برای Blade template** قابل دسترسی است و به props صفحه جاوااسکریپت منتقل نمی‌شود.
    - این برای اطلاعاتی مثل تگ‌های متا، SEO یا داده‌هایی که نیاز نیست در SPA کلاینت باشند، کاربرد دارد.
پس در عمل:
- شما Blade را به عنوان **فریم ورک اصلی HTML** دارید.
- صفحات Vue/React/Svelte به عنوان SPA درون همان Blade اجرا می‌شوند.
- می‌توانید هم داده‌ها را برای Blade داشته باشید و هم props برای صفحات Inertia ارسال کنید، بدون اینکه تداخل ایجاد شود.
### کاربردها
تمام کاربردهای **Root template data** در Inertia را می‌توان به چند دسته اصلی تقسیم کرد. این قابلیت به شما اجازه می‌دهد داده‌هایی را به **Root Blade template** بدهید که یا با صفحه جاوااسکریپت به اشتراک گذاشته می‌شوند و یا فقط مخصوص Blade هستند.
### 1. افزودن متا تگ‌ها برای SEO و شبکه‌های اجتماعی
- اضافه کردن **meta description** برای موتورهای جستجو.
- تولید **Twitter card meta tags** برای نمایش مناسب در توییتر.
- تولید **Facebook Open Graph meta tags** برای اشتراک‌گذاری لینک‌ها در فیسبوک.
- این داده‌ها از طریق `$page['props']` یا `withViewData` قابل دسترسی هستند.
### 2. تنظیم داده‌های ثابت یا پیش‌فرض برای تمام صفحات
- می‌توان داده‌هایی مثل **نام سایت، لوگو، favicon یا تنظیمات عمومی** را در Root Blade template قرار داد.
- این داده‌ها می‌توانند در تمام صفحات SPA بدون نیاز به ارسال مجدد props قابل استفاده باشند.
### 3. داده‌هایی که فقط برای Blade مورد نیاز هستند
- برخی اطلاعات نیاز است فقط در HTML سمت سرور موجود باشد و به صفحه جاوااسکریپت ارسال نشود، مثل:
    - داده‌های مربوط به SEO
    - کدهای تحلیلی (Analytics) یا تگ‌های tracking
    - داده‌های امنیتی یا تنظیمات خاص که نباید در کلاینت دیده شوند
- این کار با متد `withViewData` انجام می‌شود.
### 4. دسترسی به props صفحه از Blade
- با متغیر `$page` می‌توان داده‌های ارسال شده به صفحه جاوااسکریپت را در Blade استفاده کرد.
- مثال: نمایش عنوان یا اطلاعات خاص یک event در تگ متا:
```blade
<meta name="twitter:title" content="{{ $page['props']['event']->title }}">
```
### 5. ترکیب داده‌های Blade و SPA
- می‌توان داده‌هایی که برای Blade مهم هستند (مثلاً SEO) و داده‌هایی که برای SPA مهم هستند (props صفحه) را **همزمان ارسال کرد**.
- این کار باعث می‌شود صفحه هم برای موتورهای جستجو بهینه باشد و هم SPA بدون مشکل عمل کند.
### 6. استفاده در قالب‌های پایه (Layout)
- Root template data می‌تواند به Layoutها منتقل شود و در تمام صفحات مشترک استفاده شود.
- مثال: منوی اصلی، هدر یا فوتر که باید اطلاعاتی مثل نام کاربر یا نوتیفیکیشن‌ها را نمایش دهند.
اگر بخواهید، می‌توانم یک **جدول کاربردها و مثال‌های عملی** برای Root template data آماده کنم تا در جزوه‌تان راحت‌تر استفاده شود.
## Maximum response size
### توضیح
برای فعال کردن **تاریخچه ناوبری سمت کلاینت**، تمام پاسخ‌های سرور Inertia در **history state مرورگر** ذخیره می‌شوند.
### نکات مهم
- برخی مرورگرها محدودیت اندازه برای داده‌های ذخیره‌شده در history state دارند.
- مثال: **Firefox** محدودیت 16 MiB دارد و اگر از این حد عبور کنید، خطای `NS_ERROR_ILLEGAL_VALUE` رخ می‌دهد.
- معمولاً این مقدار بسیار بیشتر از نیاز واقعی برنامه‌ها است و در اکثر پروژه‌ها مشکلی ایجاد نمی‌کند.
### نکته عملی
- بهتر است هنگام ارسال props به صفحات، فقط **داده‌های ضروری و حداقلی** را برگردانید تا حجم پاسخ کاهش یابد و از مشکلات احتمالی جلوگیری شود.
- داده‌های بزرگ یا فایل‌ها نباید به صورت مستقیم از طریق Inertia props ارسال شوند؛ برای این موارد بهتر است از لینک‌های دانلود یا API جداگانه استفاده شود.