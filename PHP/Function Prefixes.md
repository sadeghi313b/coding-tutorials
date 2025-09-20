با سلام
در PHP، قبل از تعریف `function` می‌توانیم «پیشوندها» یا **modifiers** مختلفی قرار بدهیم که هرکدام معنا و کاربرد خاصی دارند. این‌ها بیشتر در **OOP (برنامه‌نویسی شی‌گرا)** استفاده می‌شوند.

## 🔹 دسته‌بندی کامل پیشوندهای تابع و متد در PHP
### 1. **دسترسی (Access Modifiers)**
- **`public`**
    - متد یا پراپرتی در همه جا قابل دسترسی است (داخل کلاس، کلاس‌های فرزند و بیرون کلاس).
    - پیش‌فرض است. یعنی اگر چیزی ننویسید → `public` در نظر گرفته می‌شود.
- **`protected`**
    - فقط در همان کلاس و کلاس‌های فرزند قابل دسترسی است.
    - از بیرون کلاس نمی‌توان به آن دسترسی داشت.
- **`private`**
    - فقط در همان کلاس قابل دسترسی است.
    - حتی کلاس‌های فرزند هم به آن دسترسی ندارند.
---
### 2. **ویژگی‌های مرتبط با شیء یا کلاس**
- **`static`**
    - متد یا پراپرتی به کلاس تعلق دارد نه به یک شیء (instance).
    - یعنی بدون ساختن شیء می‌توانید صدا بزنید:
        ```php
        User::methodName();
        ```
    - داخل متد استاتیک، `$this` در دسترس نیست (چون شیء وجود ندارد).
- **`final`**
    - اگر روی یک کلاس بگذارید → آن کلاس دیگر قابل ارث‌بری نیست.
    - اگر روی یک متد بگذارید → آن متد دیگر در کلاس فرزند قابل بازنویسی (override) نیست.
- **`abstract`**
    - فقط در کلاس‌های abstract استفاده می‌شود.
    - متد بدنه ندارد (فقط تعریف می‌شود) و کلاس‌های فرزند باید آن را پیاده‌سازی کنند.
---
### 3. **ویژگی‌های متدهای خاص**
- **`__construct`** (سازنده)  
    متدی است که هنگام ساختن شیء (`new ClassName`) به صورت خودکار اجرا می‌شود.
- **`__destruct`** (تخریب‌گر)  
    متدی است که هنگام نابودی شیء (مثلاً پایان اسکریپت یا unset) اجرا می‌شود.
- **`__invoke`**  
    امکان می‌دهد که یک شیء مثل یک تابع صدا زده شود.
---
## 🔹 اگر modifiers نداشته باشیم؟
اگر قبل از `function` هیچ چیزی ننویسیم:
- داخل کلاس → پیش‌فرض **`public`** در نظر گرفته می‌شود.
- بیرون از کلاس (تابع سراسری) → modifiers مجاز نیستند.
---
## 🔹 پاسخ به سناریوهای شما
### 1. برای متدی که بخواهیم مثل `User::methodName` استفاده کنیم
- باید **`static`** باشد.
- چون متد استاتیک بدون ساختن شیء قابل فراخوانی است.
    ```php
    class User {
        public static function hello() {
            return "Hello!";
        }
    }
    echo User::hello(); // کار می‌کند
    ```
اگر `static` نباشد، باید شیء بسازید:
```php
$user = new User();
$user->hello();
```
---
### 2. برای ساخت **Helper**
- معمولاً **تابع global** تعریف می‌شود (خارج از کلاس، بدون public/private).
- یا اگر در کلاس می‌سازید و می‌خواهید بدون ساخت شیء استفاده کنید → **`public static`**.
مثلاً:
```php
if (!function_exists('formatDate')) {
    function formatDate($date) {
        return date('Y-m-d', strtotime($date));
    }
}
```
یا:
```php
class Helper {
    public static function formatDate($date) {
        return date('Y-m-d', strtotime($date));
    }
}
```
---
با سلام

سؤال خیلی خوبی پرسیدی 👌  
ببین تفاوت **static method** و **instance method** در enum اینجاست:

---

### 1. متد **instance** مثل `public function label()`

- نیاز دارد که یک نمونه (case) از enum داشته باشی.
    
- یعنی اول بگویی `OrderStatus::Active` و بعد روی آن نمونه صدا بزنی:
    

```php
$status = OrderStatus::Active;
echo $status->label(); // باید نمونه داشته باشی
```

اینجا منطقی است چون label به **مقدار فعلی enum** بستگی دارد (`$this`).  
اگر استاتیک باشد دیگر `$this` در دسترس نیست.

---

### 2. متد **static**

- متدی است که برای کار روی **کل enum** نوشته می‌شود، نه روی یک case خاص.
    
- مثلاً وقتی می‌خواهی همه مقادیر یا همه labels را یکجا برگردانی:
    

```php
public static function labels(): array
{
    return [
        self::Active->value   => self::Active->label(),
        self::Force->value    => self::Force->label(),
        self::Hold->value     => self::Hold->label(),
        self::Canceled->value => self::Canceled->label(),
        self::Enough->value   => self::Enough->label(),
    ];
}
```

و استفاده:

```php
$allLabels = OrderStatus::labels();
// خروجی: ['active' => 'Active', 'force' => 'Force', ...]
```
### Summary

```php ln=false title=
public function fnc() { ..$this.. } // $instnc = clss::mthd|prpt //Gender::Male
public static function fnc() { alls } //no $this
```
