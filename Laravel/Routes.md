```php ln=false
// Define a route with parameters, default values, and optional parameter
Route::get('/users/{id}/{name?}/{status?}', [UserController::class, 'show'])
    ->defaults('name', 'Guest') // Set default value for name
    ->defaults('status', null); // Set default null for status
Route::get('/users/{id}/{name?}/{status?}', 
			function (string $id, ?string $name = 'Guest', ?string $status = null) { ... }
```
- علامت ? در <span dir=ltr>{status?}</span> در تعریف مسیر به این معناست که پارامتر status اختیاری است. به عبارت دیگر، این پارامتر می‌تواند در URL وجود نداشته باشد، و اگر غایب باشد، Laravel خطای 404 تولید نمی‌کند و مقدار پیش‌فرض (در اینجا null) برای آن استفاده می‌شود.
- علامت ? در <span dir=ltr>?string</span> بخشی از نوع‌دهی (type hinting) در PHP است و به این معناست که پارامتر می‌تواند یک رشته (string) یا null باشد. این نوع‌دهی در PHP 7.1 و بالاتر پشتیبانی می‌شود و به نام nullable type شناخته می‌شود.
```php ln=false title=
//web.php
Route::delete('users/{user}/bulk-destroy',[UserController::class, 'bulkDestroy'])
​	->name('users.bulkDestroy');
//Page.vue
router.delete( route('users.bulkDestroy', {id:5} ) );
```
تابع `route()` فقط برای ساختن **URL با پارامترهای مسیر** است، نه برای ارسال payload.

```php title=
Route::delete('users/{user}', [UserController::class, 'destroy'])->name('users.destroy');
Route::delete('users/bulk-destroy',[UserController::class, 'bulkDestroy'])
​	->name('users.bulkDestroy');
```
خط 1 میتواند زودتر match شود و اجازه ندهد جریان برنامه به خط 2 برسد. مسیرهای **Static** (مثل `/users/bulk`) باید **قبل از مسیرهای داینامیک** (مثل `/users/{user}`) تعریف شوند، وگرنه روت داینامیک همیشه اول match می‌شود.
**مسیرهای داینامیک (`{param}`) همه چیز بعد از `/prefix/` را می‌گیرند** و اگر قبل از مسیرهای استاتیک (`/bulk`) تعریف شوند، مسیر استاتیک هرگز match نمی‌شود. می‌توانی محدودیت **Regex** روی پارامتر داینامیک بگذاری تا فقط عدد یا فرمت مشخصی بگیرد و `/bulk` match نشود:
```php ln=false title=
Route::delete('users/{user}', [UserController::class, 'destroy'])
    ->whereNumber('user')  //  فقط اعداد یا مدل بایندینگ
    ->name('users.destroy');

```



