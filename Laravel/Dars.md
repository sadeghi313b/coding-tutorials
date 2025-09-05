# Resource controller
در لاراول وقتی از **Resource Collection** استفاده می‌کنید (مثل `StudentResource::collection(...)`)، خروجی به شکل زیر برمی‌گردد:
```php ln=false
{
  "data": [
    { "id": 1, "name": "John", ... },
    { "id": 2, "name": "Sara", ... }
  ]
}
```
به همین دلیل شما باید در Vue از `students.data` استفاده کنید، چون آبجکت اصلی یک wrapper است که داخلش کلید `data` وجود دارد. 
<p style="line-height: 0.2rem;"></p>
اگر بخواهید مستقیماً از `students` استفاده کنید (بدون `.data`)، می‌توانید تبدیل به آرایه ساده در کنترلر کنید:
```php ln=false
$students = StudentResource::collection(Student::all())->resolve();
```

pass to vue components: \<Pagination :data="students" />
# Nullsafe operator
**علامت سؤال `?->`** همون **Nullsafe operator** در PHP (از نسخه 8 به بعد) هست.
- اگر `$this->created_at` مقدار داشته باشه (یعنی null نباشه):  متد `toFormattedDateString()` روی اون شیء صدا زده میشه.
 - اگر `$this->created_at` مقدار null باشه:  هیچ خطایی نمی‌ده و کل عبارت `null` برمی‌گردونه.
 - 
# pagination
 server side pagination vs client side pagination
 http://127.0.0.1:8000/students?page=2
 pagination html source in tailwind or laravel websites
# Object
type: object, //Error
type: Object, //True





---
when label: "&laquo; Previous"    then do: \<span v-html="link.lable"></span>

---
vue class
:disabled="link.active || !link.url"

---
روال: متد در کنترلر -> page در vue

---
## Ziggy چیست؟
در واقع **Ziggy** یک پکیج (package) برای **Laravel** است که  اجازه  می‌دهد از **Routeهای نام‌گذاری‌شده لاراول** در سمت **JavaScript/Vue/React** هم استفاده کنیم.
### مشکل بدون Ziggy
- در **Laravel Blade** به راحتی می‌توانیم از `route('students.index')` استفاده کنیم.
- ولی اگر در **JavaScript** یا **Vue** مستقیماً `route('students.index')` را بنویسیم، مرورگر آن را نمی‌شناسد (چون تابع `route()` فقط در PHP وجود دارد).
### راه‌حل Ziggy
Ziggy این مشکل را حل می‌کند:
1. وقتی Ziggy را نصب می‌کنیم، تمام **Routeهای تعریف‌شده در Laravel** را به فرانت‌اند (JavaScript) منتقل می‌کند.
2. یک تابع جاوااسکریپتی `route()` در اختیارمان می‌گذارد که دقیقاً مثل نسخه‌ی PHP کار می‌کند.
3. می​​​​﻿⁠‌خورم

## web.app
```php  ln=false
<\?php 

use Illuminate\Support\Facades\Route;
use App\Http\Controllers\UserController;

/*
|--------------------------------------------------------------------------
| Web Routes – each route declared explicitly
|--------------------------------------------------------------------------
*/

// CRUD
Route::get ('/users',[UserController::class, 'index'  ]) ->name('users.index');    // list
Route::get   ('/users/create',       [UserController::class, 'create' ])->name('users.create');   // form
Route::post  ('/users',              [UserController::class, 'store'  ])->name('users.store');    // save
Route::get   ('/users/{user}',       [UserController::class, 'show'   ])->name('users.show');     // single
Route::get   ('/users/{user}/edit',  [UserController::class, 'edit'   ])->name('users.edit');     // form
Route::match (['put', 'patch'], '/users/{user}', [UserController::class, 'update' ])->name('users.update');   // update
Route::delete('/users/{user}',       [UserController::class, 'destroy'])->name('users.destroy');  // soft‑delete

// Soft‑delete extras
Route::get   ('/users/trashed',          [UserController::class, 'trashed'   ])->name('users.trashed');
Route::patch ('/users/{id}/restore',     [UserController::class, 'restore'   ])->name('users.restore');
Route::delete('/users/{id}/force',       [UserController::class, 'forceDelete'])->name('users.force');
```

# Database
## Factory
با متد definition خروجی متداول و پیش فرض فکتوری مشخص می شود:
```php ln=false
public function definition(): array    {        
	return [            
		'name' => fake()->name(),            
		'email' => fake()->unique()->safeEmail(),            
		'email_verified_at' => now(),            
		'password' => static::$password ??= Hash::make('password'),            
		'remember_token' => Str::random(10),        
	];    
}
```
The `definition` method returns the default set of attribute values.
array $attributes: \[name,email, email_verified_at, password, remember_token]

## Factory States[:](https://laravel.com/docs/12.x/eloquent-factories#factory-states)
مواردیکه میخواهیم برخی اتریبیوت ها مقادیری غیر از حالت پیشفرض بگیرند باید متدهایی غیر از definition نیز بنویسیم و سپس این متدها را نیز در هنگام فراخوانی فکتوری با آن chain کنیم. مثلا تولید یوزری که معلق شده باشد:
```php ln=false
public function suspended(): Factory {    
	return $this->state(function (array $attributes) {        
		return [
			'account_status' => 'suspended',
		];
	});
}
```

در هنگام فراخوانی مثلا در seeder:
```php ln=false
$user = User::factory()->suspended()->create();
$user = User::factory()->trashed()->create();
```
متد trashed یک متد درونی در لاراول است که اتریبیوت soft-delete را طوری تنظیم میکند که رکورد soft delete شده تولید شود.