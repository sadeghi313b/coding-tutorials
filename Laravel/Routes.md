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


