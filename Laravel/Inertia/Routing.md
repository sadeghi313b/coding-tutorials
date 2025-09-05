## Defining routes
### توضیح
هنگام استفاده از **Inertia**، تمام **مسیرهای برنامه (routes)** در سمت سرور تعریف می‌شوند.
- نیازی به استفاده از **Vue Router** یا **React Router** نیست.
- کافی است **Laravel routes** تعریف کنید و از آنها **Inertia responses** برگردانید.
### نکته عملی
- مدیریت مسیرها و کنترل دسترسی‌ها تنها در سمت سرور انجام می‌شود.
- صفحات SPA به صورت خودکار بر اساس مسیرهای لاراول رندر می‌شوند و نیازی به مسیرهای کلاینت جداگانه نیست.
## Shorthand routes
### توضیح
اگر صفحه‌ای دارید که **نیازی به متد کنترلر ندارد**، مثل صفحات "FAQ" یا "About"، می‌توانید مستقیماً آن را به کامپوننت روت کنید.
- برای این کار از متد `Route::inertia()` استفاده می‌شود.
- این روش کوتاه و ساده است و نیازی به ایجاد کنترلر ندارد.
### Syntax
```php
// File: routes/web.php
// This defines a direct Inertia route for a page without a controller
Route::inertia('/about', 'About');
```
- و `'/about'`: مسیر URL که صفحه در آن قابل دسترسی است.
- و `'About'`: نام کامپوننت صفحه در پروژه جاوااسکریپت (Vue/React/Svelte).
- این صفحه به محض دسترسی کاربر رندر می‌شود و props اضافی نیز می‌توانند با آرایه دوم به آن منتقل شوند.
## Generating URLs
### توضیح
در برخی فریمورک‌های سرور-ساید می‌توان با **named routes** آدرس‌ها را تولید کرد، اما این قابلیت به صورت پیش‌فرض **در سمت کلاینت در دسترس نیست**.
و Inertia دو راهکار برای این مشکل ارائه می‌دهد:
### 1. تولید URL سرور-ساید و ارسال به صفحه
- می‌توانید URLها را سرور-ساید تولید کرده و به عنوان **prop** به صفحه ارسال کنید.
- مثال زیر نحوه ارسال `edit_url` و `create_url` به کامپوننت `Users/Index` را نشان می‌دهد:
```php
// UsersController.php: Controller that generates URLs as props for the Users/Index page
class UsersController extends Controller
{
    public function index()
    {
        return Inertia::render('Users/Index', [
            'users' => User::all()->map(function ($user) {
                return [
                    'id' => $user->id,
                    'name' => $user->name,
                    'email' => $user->email,
                    'edit_url' => route('users.edit', $user), // URL for editing this user
                ];
            }),
            'create_url' => route('users.create'), // URL for creating a new user
        ]);
    }
}
```
### 2. استفاده از کتابخانه Ziggy
- و **Ziggy** امکان استفاده از **named routes لاراول** در سمت کلاینت را فراهم می‌کند.
- در پروژه‌هایی که از **Laravel starter kits** استفاده می‌کنند، Ziggy معمولاً از قبل نصب و تنظیم شده است.
```vue
// Index.vue: Vue page component using Ziggy for client-side named routes
<Link :href="route('users.create')">Create User</Link>
```
- و `route()` می‌تواند مستقیماً در قالب‌های Vue استفاده شود.
```js
// ssr.js: SSR configuration for Ziggy plugin, passing routes and current location
.use(ZiggyVue, {
  ...page.props.ziggy,
  location: new URL(page.props.ziggy.location),
});
```
- این کار تضمین می‌کند که **تعریف مسیرها و موقعیت فعلی** برای Server-side rendering نیز در دسترس باشد.
### نکات
- روش اول برای پروژه‌های کوچک و ساده مناسب است.
- روش دوم با Ziggy برای SPAهای بزرگ و پیچیده کاربردی است و نیاز به ارسال دستی URLها را حذف می‌کند.
### Generating URLs with Ziggy – Syntax
اگر می‌خواهید در سمت کلاینت از **Ziggy** برای دسترسی به **named routes** لاراول استفاده کنید، سینتکس کلی به این شکل است:
```js
// Import ZiggyVue and install in your app
import { ZiggyVue } from 'ziggy';
app.use(ZiggyVue, {
  ...page.props.ziggy,                 // Pass route definitions from server
  location: new URL(page.props.ziggy.location), // Current URL
});
```
در قالب Vue:
```vue
// Using named route in a template
<Link :href="route('route.name', { param1: value1, param2: value2 })">
  Link Text
</Link>
```
- `route('route.name')`: نام مسیر لاراول که در سرور تعریف شده است.
- `{ param1: value1 }`: آرایه‌ای از پارامترهای مسیر، در صورت نیاز.
- خروجی: URL کامپایل شده که می‌تواند مستقیماً در href یا سایر مکان‌ها استفاده شود.
- این روش **امکان استفاده از مسیرهای سرور در سمت کلاینت** را بدون ارسال دستی URLها فراهم می‌کند.
## Customizing the Page URL
### توضیح
در فریم‌ورک **Inertia.js** که با **Laravel** استفاده می‌شه، قابلیت **Customizing the Page URL** به شما امکان می‌ده نحوه تولید URL برای شیء `page` رو شخصی‌سازی کنید. این قابلیت برای کنترل دقیق‌تر روی URLهایی که Inertia به سمت کلاینت (مثل Vue یا React) ارسال می‌کنه، مفیده. 
### توضیح مفهوم
- **شیء page در Inertia**:
  - وقتی Inertia یک صفحه رو رندر می‌کنه، یک شیء JSON به نام `page` به کلاینت ارسال می‌شه که شامل اطلاعاتی مثل داده‌های صفحه (props)، نام کامپوننت، و **URL** فعلی صفحه است.
  - این URL به‌طور پیش‌فرض با استفاده از متد `fullUrl()` از نمونه `Request` در Laravel تولید می‌شه، اما **طرح (scheme)** و **هاست (host)** حذف می‌شن تا URL به‌صورت **نسبی (relative)** باشه (مثلاً `/dashboard` به‌جای `https://example.com/dashboard`).
- **چرا شخصی‌سازی URL؟**:
  - ممکنه بخواید URL رو به شکلی خاص تولید کنید، مثلاً:
    - حذف یا اضافه کردن پارامترهای کوئری.
    - تغییر مسیر پایه (base path).
    - استفاده از ساختار URL متفاوت برای محیط‌های خاص (مثل API یا چندزبانه).
  - این شخصی‌سازی برای هماهنگی با نیازهای پروژه یا بهینه‌سازی رفتار کلاینت‌ساید (مثل تاریخچه مرورگر) کاربرد داره.
- **دو روش برای شخصی‌سازی**:
  1. استفاده از متد `urlResolver` در میدلور `HandleInertiaRequests`.
  2. استفاده از متد استاتیک `Inertia::resolveUrlUsing()`.
### روش 1: استفاده از متد `urlResolver` در میدلور
شما می‌تونید در کلاس میدلور `HandleInertiaRequests` (که معمولاً در مسیر `app/Http/Middleware/HandleInertiaRequests.php` قرار داره) متد `urlResolver` رو بازنویسی کنید تا URL دلخواه تولید بشه.
**نمونه کد**:
```php
<?php
// app/Http/Middleware/HandleInertiaRequests.php
namespace App\Http\Middleware;
use Illuminate\Http\Request;
use Inertia\Middleware;
class HandleInertiaRequests extends Middleware
{
    /**
     * Resolve the URL for the current page.
     *
     * @param  \Illuminate\Http\Request  $request
     * @return string
     */
    public function urlResolver()
    {
        return function (Request $request) {
            // Custom logic to generate the URL
            $url = $request->path(); // Example: Return only the path (e.g., /dashboard)
            $query = $request->query(); // Get query parameters if needed
            if (!empty($query)) {
                $url .= '?' . http_build_query($query); // Append query parameters
            }
            return $url;
        };
    }
}
```
**توضیحات کد**:
- **متد `urlResolver`**:
  - یک کلوژر برمی‌گردونه که نمونه `Request` رو دریافت می‌کنه.
  - شما می‌تونید منطق دلخواه برای تولید URL رو پیاده‌سازی کنید.
- **مثال بالا**:
  - از `$request->path()` برای گرفتن مسیر نسبی (بدون هاست و طرح) استفاده می‌شه.
  - پارامترهای کوئری (query parameters) رو با `http_build_query` اضافه می‌کنه.
  - نتیجه مثلاً `/dashboard?filter=active` خواهد بود.
#### استفاده عملی
- فرض کنید می‌خواهید همیشه پارامتر خاصی (مثل `lang=fa`) رو به URL اضافه کنید:
  ```php
  public function urlResolver()
  {
      return function (Request $request) {
          $url = $request->path();
          $query = array_merge($request->query(), ['lang' => 'fa']);
          return $url . '?' . http_build_query($query);
      };
  }
  ```
  - خروجی: `/dashboard?filter=active&lang=fa`
### روش 2: استفاده از `Inertia::resolveUrlUsing`
این روش به شما امکان می‌ده بدون تغییر میدلور، یک resolver سراسری برای URL تعریف کنید. این کار معمولاً در فایل `AppServiceProvider` یا فایل راه‌اندازی Inertia (مثل `app/Providers/InertiaServiceProvider.php`) انجام می‌شه.
**نمونه کد**:
```php
<?php
// app/Providers/AppServiceProvider.php
namespace App\Providers;
use Illuminate\Support\ServiceProvider;
use Inertia\Inertia;
class AppServiceProvider extends ServiceProvider
{
    /**
     * Bootstrap any application services.
     */
    public function boot()
    {
        // Define custom URL resolver for Inertia
        Inertia::resolveUrlUsing(function (\Illuminate\Http\Request $request) {
            // Custom logic to generate the URL
            $url = $request->path();
            $query = $request->query();
            if (!empty($query)) {
                $url .= '?' . http_build_query($query);
            }
            return $url;
        });
    }
}
```
**توضیحات کد**:
- **متد `Inertia::resolveUrlUsing`**:
  - یک کلوژر می‌گیره که نمونه `Request` رو دریافت می‌کنه و URL دلخواه رو برمی‌گردونه.
  - این روش به‌صورت سراسری اعمال می‌شه و نیازی به تغییر میدلور نداره.
- **مثال بالا**:
  - مشابه روش اول، مسیر نسبی و پارامترهای کوئری رو ترکیب می‌کنه.
  - می‌تونید منطق رو تغییر بدید، مثلاً فقط مسیرهای خاصی رو برگردونید یا پارامترهای اضافی اضافه کنید.
#### استفاده عملی
- فرض کنید می‌خواهید URL رو بدون پارامترهای خاصی (مثل `token`) برگردونید:
  ```php
  Inertia::resolveUrlUsing(function (\Illuminate\Http\Request $request) {
      $url = $request->path();
      $query = array_diff_key($request->query(), ['token' => null]); // Remove 'token' parameter
      if (!empty($query)) {
          $url .= '?' . http_build_query($query);
      }
      return $url;
  });
  ```
  - خروجی: اگر درخواست `/dashboard?filter=active&token=xyz` باشه، نتیجه `/dashboard?filter=active` خواهد بود.
### نکات کلیدی
- **تفاوت دو روش**:
  - `urlResolver` در میدلور فقط برای آن میدلور اعمال می‌شه و مناسب پروژه‌هایی است که چندین میدلور Inertia دارن.
  - `Inertia::resolveUrlUsing` سراسری است و برای همه درخواست‌های Inertia اعمال می‌شه، بنابراین ساده‌تره اگر پروژه شما یک میدلور داره.
- **موارد استفاده**:
  - حذف یا اضافه کردن پارامترهای کوئری (مثل چندزبانه کردن URL).
  - تغییر مسیر پایه برای SPA (مثلاً برای زیرپوشه‌ها).
  - هماهنگ‌سازی URL با نیازهای کلاینت‌ساید (مثل تاریخچه مرورگر).
- **دیباگ**:
  - برای تست URL تولیدشده، می‌تونید در کلوژر یک `Log::info($url)` اضافه کنید:
    ```php
    Inertia::resolveUrlUsing(function (\Illuminate\Http\Request $request) {
        $url = $request->path();
        \Illuminate\Support\Facades\Log::info('Resolved URL: ' . $url);
        return $url;
    });
    ```
موضوع **Customizing the Page URL** در Inertia.js با Laravel به شما امکان می‌دهد نحوه تولید URL در شیء `page` را که به کلاینت (مثل Vue یا React) ارسال می‌شود، کنترل کنید. این قابلیت کاربردهای متنوعی داره که می‌تونه به بهبود تجربه کاربری، مدیریت چندزبانه، یا هماهنگی با نیازهای خاص پروژه کمک کنه. در زیر انواع کاربردهای این قابلیت رو به‌صورت خلاصه و به فارسی توضیح می‌دم:
### کاربردهای Customizing the Page URL
1. **مدیریت پارامترهای کوئری (Query Parameters)**:
   - **کاربرد**: حذف یا اضافه کردن پارامترهای کوئری خاص در URL ارسالی به کلاینت.
   - **مثال**: حذف پارامترهای حساس مثل `token` یا `session_id` از URL.
     ```php
     Inertia::resolveUrlUsing(function (\Illuminate\Http\Request $request) {
         // Remove sensitive query parameters
         $url = $request->path();
         $query = array_diff_key($request->query(), ['token' => null]);
         return $url . (empty($query) ? '' : '?' . http_build_query($query));
     });
     ```
   - **فایده**: افزایش امنیت یا ساده‌سازی URL برای کاربر.
2. **پشتیبانی از چندزبانه (Localization)**:
   - **کاربرد**: افزودن پارامتر زبان (مثل `lang=fa`) به URL برای هماهنگی با سیستم چندزبانه.
   - **مثال**: اضافه کردن زبان کاربر به URL.
     ```php
     Inertia::resolveUrlUsing(function (\Illuminate\Http\Request $request) {
         // Add language parameter to URL
         $url = $request->path();
         $query = array_merge($request->query(), ['lang' => app()->getLocale()]);
         return $url . '?' . http_build_query($query);
     });
     ```
   - **فایده**: اطمینان از نمایش محتوای مناسب با زبان کاربر در کلاینت.
3. **تغییر مسیر پایه (Base Path)**:
   - **کاربرد**: تنظیم URL برای پروژه‌هایی که در زیرپوشه‌ها (subdirectory) میزبانی می‌شن.
   - **مثال**: اگر اپلیکیشن در `example.com/app` میزبانی می‌شه، مسیر پایه رو اضافه کنید.
     ```php
     Inertia::resolveUrlUsing(function (\Illuminate\Http\Request $request) {
         // Prepend base path to URL
         return '/app' . $request->fullUrl();
     });
     ```
   - **فایده**: هماهنگی با سرورهای غیراستاندارد یا تنظیمات خاص.
4. **مدیریت تاریخچه مرورگر (Browser History)**:
   - **کاربرد**: کنترل URL برای به‌روزرسانی صحیح تاریخچه مرورگر در SPA.
   - **مثال**: تغییر فرمت URL برای جلوگیری از تکرار پارامترها در history.
     ```php
     Inertia::resolveUrlUsing(function (\Illuminate\Http\Request $request) {
         // Simplify URL for browser history
         return $request->path();
     });
     ```
   - **فایده**: بهبود تجربه کاربری با جلوگیری از URLهای پیچیده در تاریخچه.
5. **سازگاری با APIهای خارجی**:
   - **کاربرد**: تولید URLهایی که با APIهای خارجی یا سیستم‌های دیگر هماهنگ باشن.
   - **مثال**: تبدیل مسیرهای داخلی به فرمت API.
     ```php
     Inertia::resolveUrlUsing(function (\Illuminate\Http\Request $request) {
         // Map internal routes to API endpoints
         $url = str_replace('dashboard', 'api/dashboard', $request->path());
         return $url;
     });
     ```
   - **فایده**: ادغام بهتر با سیستم‌های خارجی.
6. **شخصی‌سازی برای SEO یا Analytics**:
   - **کاربرد**: تولید URLهایی که برای ابزارهای SEO یا آنالیتیکس بهینه باشن.
   - **مثال**: افزودن پارامترهای ردیابی (tracking) به URL.
     ```php
     Inertia::resolveUrlUsing(function (\Illuminate\Http\Request $request) {
         // Add tracking parameters for analytics
         $url = $request->path();
         $query = array_merge($request->query(), ['utm_source' => 'inertia']);
         return $url . '?' . http_build_query($query);
     });
     ```
   - **فایده**: کمک به ردیابی رفتار کاربران در ابزارهای آنالیتیکس.
7. **مدیریت محیط‌های مختلف (Production/Development)**:
   - **کاربرد**: تولید URLهای متفاوت برای محیط‌های توسعه و تولید.
   - **مثال**: افزودن پیشوند برای محیط توسعه.
     ```php
     Inertia::resolveUrlUsing(function (\Illuminate\Http\Request $request) {
         // Add environment-specific prefix
         $prefix = app()->environment('production') ? '' : '/dev';
         return $prefix . $request->fullUrl();
     });
     ```
   - **فایده**: جلوگیری از تداخل در محیط‌های مختلف.
### نکات
- **انتخاب روش**: اگر نیاز به کنترل در سطح میدلور دارید، از `urlResolver` در `HandleInertiaRequests` استفاده کنید. برای تغییرات سراسری، `Inertia::resolveUrlUsing` مناسب‌تره.
- **دیباگ**: از `Log::info($url)` در کلوژر استفاده کنید تا URL تولیدشده رو بررسی کنید.
- **محدودیت‌ها**: تغییر URL فقط روی شیء `page` در Inertia تأثیر می‌ذاره و نباید ساختار مسیریابی Laravel رو مختل کنه.
اگر نیاز به مثال کد برای کاربرد خاصی (مثل چندزبانه یا SEO) یا توضیحات بیشتر دارید، بفرمایید!