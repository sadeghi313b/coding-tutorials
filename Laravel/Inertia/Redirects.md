## Redirects
### توضیح
هنگام انجام یک درخواست **غیر GET** در Inertia (مثلاً POST، PUT، DELETE) چه به صورت دستی و چه از طریق `<Link>`، باید اطمینان حاصل کنید که پاسخ **Redirect مناسب Inertia** باشد.
- این کار باعث می‌شود که Inertia به طور خودکار **Redirect را دنبال کند** و صفحه را به‌روزرسانی کند.
- معمولاً بعد از ایجاد یا ویرایش داده، باید کاربر را به یک **GET endpoint استاندارد** هدایت کنید، مثل صفحه index یا صفحه جزئیات.
### Syntax کلی
```php
// Redirect to a named route
return to_route('route.name');
// Or redirect to a URL
return redirect('/some-url');
```
- `to_route('route.name')`: هدایت به یک **route مشخص** با نام route.
- `redirect('/some-url')`: هدایت به یک **آدرس URL خاص**.
### مثال در Laravel
```php
// This is a Laravel controller demonstrating proper Inertia redirects
class UsersController extends Controller
{
    public function index()
    {
        return Inertia::render('Users/Index', [
            'users' => User::all(),
        ]);
    }
    public function store(Request $request)
    {
        // Validate and create a new user
        User::create(
            $request->validate([
                'name' => ['required', 'max:50'],
                'email' => ['required', 'max:50', 'email'],
            ])
        );
        // Redirect to the index page
        return to_route('users.index');
    }
}
```
### نکات مهم
- استفاده از `to_route()` یا `redirect()->route()` باعث می‌شود Inertia **Redirect را به صورت SPA مدیریت کند** و صفحه را بدون رفرش کامل مرورگر به‌روزرسانی کند.
- اطمینان حاصل کنید که **endpoint مقصد یک GET request معتبر است**.
- این روش باعث حفظ تجربه کاربری یک SPA می‌شود و نیازی به رفرش صفحه نیست.
## 303 response code
### توضیح
هنگام انجام **Redirect** بعد از یک درخواست **PUT، PATCH یا DELETE** باید از **کد وضعیت 303** استفاده کنید.
- دلیل: اگر از 303 استفاده نکنید، درخواست بعدی به عنوان GET در نظر گرفته نمی‌شود.
- و Redirect با کد 303 شبیه 302 است، اما تفاوت اصلی این است که **درخواست بعدی به صراحت به GET تغییر می‌کند**.
### نکته عملی
- اگر از یکی از **سرور-ساید Adapterهای رسمی Inertia** استفاده کنید، همه Redirectها به طور خودکار به **303** تبدیل می‌شوند و نیازی به تنظیم دستی نیست.
### کاربرد
- جلوگیری از بروز مشکلات هنگام ریدایرکت بعد از ویرایش یا حذف داده‌ها.
- تضمین می‌کند که کاربر بعد از عملیات PUT/PATCH/DELETE به صفحه GET هدایت شود و SPA به درستی به‌روزرسانی شود.
اگر از **یکی از Adapterهای رسمی سرور Inertia** (مثل Laravel adapter) استفاده می‌کنید:
- نیازی نیست کاری انجام دهید.
- همه Redirectها به صورت **خودکار به 303** تبدیل می‌شوند.
- بنابراین بعد از یک **PUT، PATCH یا DELETE**، درخواست بعدی به درستی به GET تبدیل شده و صفحه SPA بدون مشکل به‌روزرسانی می‌شود.
## External redirects
### توضیح
گاهی لازم است کاربر را به یک **وبسایت خارجی** یا یک **endpoint غیر Inertia** در برنامه هدایت کنید.  
برای این کار می‌توان از متد `Inertia::location()` استفاده کرد که یک **ریدایرکت سمت سرور** ایجاد می‌کند.
### Syntax
```php
return Inertia::location($url);
```
- `$url`: آدرس مقصد که کاربر باید به آن هدایت شود.
### نحوه کار
- متد `Inertia::location()` یک **409 Conflict response** ایجاد می‌کند و آدرس مقصد را در هدر `X-Inertia-Location` قرار می‌دهد.
- وقتی این پاسخ سمت کلاینت دریافت شود، Inertia به طور خودکار اجرای زیر را انجام می‌دهد:
```js
window.location = url
```
- بنابراین صفحه به مقصد مشخص شده هدایت می‌شود، حتی اگر مقصد یک URL خارجی یا غیر Inertia باشد.
### کاربرد
- هدایت به سایت‌های خارجی بعد از عملیات خاص.
- هدایت به مسیرهای غیر Inertia در همان برنامه، بدون اینکه SPA دچار مشکل شود.
- حفظ تجربه کاربری یکپارچه SPA هنگام نیاز به ریدایرکت خارج از Inertia.