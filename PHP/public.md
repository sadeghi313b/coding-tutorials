در PHP وقتی یک `return` با آرایه دارید، آخر آرایه باید **;** بگذارید، نه فقط `]`
return \[ 'class_id' => $section->class_id ];

در کد cdv یک `,` اضافه بعد از آرایه بود. لاراول اینو قبول نمی‌کنه و کل sequence رو نادیده می‌گیره.
sequence(fn (Sequence $sequence) => \['name' => 'class '.($sequence->index + 1)],)

---
# Operators
##  "?" Operator
### Optinal Type-Hint
```php ln=false title=
function test(?string $name) {} ≡ function test(String|null $name) {}
public function update(?User $user, Post $post): bool
```
- `User $user` : <span dir=rtl>اگر یوزر لاگین نکرده باشد، آنگاه خطای type error میدهد</span>
- `?User $user` : <span dir=rtl>اگر یوزر لاگین نکرده باشد آنگاه null برمیگرداند</span>
### Optional Return Type
```php ln=false title=
function foo(?string $name): ?int {
    return $name ? strlen($name) : null;
}

```

### Null Coalescing Operator (??) `(PHP 7.0+)`
برای گرفتن مقدار یک متغیر اگر مقدار دارد و استفاده از مقدار پیش‌فرض در غیر این صورت. فقط **بررسی می‌کند آیا متغیر **`null`** است یا نه**. اگر null بود آنگاه مقدار پیش فرض ملاک است.
```php ln=false title=
$value = $_GET['name'] ?? 'Guest'; //isset($_GET['name']) ? $_GET['name'] : 'Guest'
```
در **JS و PHP** `??` فقط به `null/undefined` حساس است، نه سایر مقادیر Falsy مثل `0` یا `''`. هر گاه مقدار اول null یا undefined بود آنگاه مقدار دوم برگردانده می‌شود.
### Ternary Operator کوتاه (?:)
نسخه کوتاه ternary برای بررسی شرط: - بررسی می‌کند آیا **مقدار متغیر "true-like"** است یا نه. یعنی null، false، 0، "" (رشته خالی) و [] (آرایه خالی) همه **false** محسوب می‌شوند.
```php ln=false title=
$value = $a ?: 'default'; // $value = $a ? $a : 'default'
```
### Nullsafe Operator (?->) `(PHP 8.0+)`
برای جلوگیری از خطای `null` هنگام دسترسی به خصوصیات یا متدها:
```php ln=false title=
$user = null;
echo $user ?-> profile ?-> email; // اگر $user یا profile null باشد، خطا نمی‌دهد و نتیجه null است
```
### array destructuring / spread و match expression
- در PHP 8+ گاهی در `match` یا الگوهای پیچیده برای null به کار می‌رود، اما موارد اصلی همان موارد بالا هستند.

---
## ... Operator
در PHP سه نقطه (`...`) **دو کاربرد اصلی** دارند:
### 1️⃣ **Spread Operator در آرایه‌ها** (PHP 7.4+)
برای باز کردن محتویات یک آرایه در آرایه‌ی دیگر.
```php ln=false title=
$array1 = ['a', 'b'];
$array2 = [...$array1, 'c'];
print_r($array2); // ['a', 'b', 'c']
```
### 2️⃣ **Variadic Functions** (آرگومان‌های متغیر)
وقتی می‌خواهیم یک تابع بتواند **تعداد دلخواهی آرگومان** دریافت کند.
```php ln=false title=
function sum(...$numbers) {
    return array_sum($numbers);
}
echo sum(1, 2, 3, 4); // 10
```
- داخل تابع، `$numbers` یک آرایه است که همه آرگومان‌ها در آن قرار می‌گیرند.
