

---
# ارسال داده به بک اند
در لاراول، وقتی داده‌ها را با `data: { ids }` به بک‌اند می‌فرستید، در کنترلر می‌توانید به چند روش به آن دسترسی داشته باشید:
1. مستقیم با نام کلید:
```php
$ids = $request->ids;
```
2. با متد `input()` و مقدار پیش‌فرض:
```php
$ids = $request->input('ids', []); // اگر کلید موجود نبود، آرایه خالی برمی‌گردد
```
تفاوت اصلی این است که `input()` به شما امکان می‌دهد مقدار پیش‌فرض تعریف کنید تا از خطا جلوگیری شود، مخصوصاً وقتی ممکن است کلید `ids` در درخواست وجود نداشته باشد.
اگر بخواهید، می‌توانم یک نمونه کامل از **متد bulkDestroy در کنترلر** با همین منطق برای حذف چند رکورد بنویسم.

---
# Access Collection
```php ln=false title=
$value = $record->get($field); // Returns null if not exists
$value = $record->get($field, 'default'); // With default value
**Dot notation support** - `get('relation.field')`

// Property Access (If attribute exists)
$value = $record->$field; // Direct property access
$value = $record->{$field}; // For dynamic field names
```

The error occurs because you're using `$record->has($field)` on an Eloquent model, but **`has()` is a relationship method**, not a field existence checker.
```php ln=false title=
//has field
property_exists($record, $field)
is_null($record->getAttribute($field))

//has relation
$record->has($field)
```


---
```php ln=false title=Objects
// initialing
$object = {}
$object = (object)[]
$object = new /stdClass
$object = json_decode('{}');

//clear an existing object
$object = new /stdClass  //re-assign to New `stdClass
$object = (object) [];
$object = json_decode('{}');

$object -> {$key} = $value;
// Overwrites existing 'name' if key exist
// or push if key does not exist

foreach ($data as $key => $value) { $object->{$key} = $value; }
$array = []; foreach ($data as $key => $value) { $array[$key] = $value; } 
​	$object = (object) $array;
foreach ($data as $key => $value) {
    if (!property_exists($object, $key)) {
        $object -> {$key} = $value;
    }
}
```

```php ln=false title=Input/Outpot
$data = ['name' => 'John', 'age' => 30, 'city' => 'New York'];

$object is now:
{
    "name": "John",
    "age": 30,
    "city": "New York"
}
```

**`stdClass`** is PHP's **generic empty class** - a built-in class that serves as a blank object for dynamic property assignment.
```php ln=false title=
$object = new \stdClass();
$object->name = 'John';
$object->age = 30;

$array = ['name' => 'Jane', 'age' => 25];
$object = (object) $array; // Creates stdClass object
```

---
# Paginator
$users = $query->paginate(5); // Returns LengthAwarePaginator object
Your code has a **critical issue**. You cannot use `find()` on a `LengthAwarePaginator` object.
```php ln=false title=SUMMARY
$paginator-> items():array | toArray():array | getCollection():collection |
```


**Pagination Methods (Chainable)**
->withPath($path)          // Set custom base path
->withQueryString()        // Include all current query string values
->appends($array)          // Add query string parameters
->fragment($fragment)      // Add URL fragment (#section)
Since the paginator contains a collection of items, you can chain collection methods by first accessing the collection:
// Method 1: Transform items then continue pagination
->through(function($item) {
    // Modify each item
    return $item;
})
// Method 2: Get collection, modify, then set back
->setCollection($collection->map(function($item) {
    // Modify items
    return $item;
}))
# 1. **Convert to Collection**
```php ln=false title=
$paginator = User::paginate(5);

// Method 1: Get the underlying collection
$collection = $paginator->getCollection();
// Returns: Illuminate\Support\Collection

// Method 2: Use items() method
$collection = collect($paginator->items());
// Returns: Illuminate\Support\Collection

// Method 3: Direct conversion
$collection = collect($paginator->items());
```
## 2. **Convert to Array**
```php ln=false title=
$paginator = User::paginate(5);

// Method 1: Get array of items only
$array = $paginator->items();
/* Returns: array of models/items
// Output:
[
    ['name' => 'John Doe', 'email' => 'john@example.com', 'phone' => '+1-555-1234567'],
    ['name' => 'Jane Smith', 'email' => 'jane@example.com', 'phone' => '+1-444-9876543'],
    ['name' => 'Bob Johnson', 'email' => 'bob@example.com', 'phone' => '+1-333-4567890']
]
*/

// Method 2: Get complete pagination data as array
$fullArray = $paginator->toArray();
/* Returns:
[
    'current_page' => 1,
    'data' => [...], // Your items here
    'first_page_url' => '...',
    'from' => 1,
    'last_page' => 3,
    'last_page_url' => '...',
    'links' => [...],
    'next_page_url' => '...',
    'path' => '...',
    'per_page' => 5,
    'prev_page_url' => null,
    'to' => 5,
    'total' => 12
]
*/

// Method 3: Get only the data array
$dataArray = $paginator->toArray()['data'];
```

<hr>

```php ln=false title=
$collection -> getAttribute() : array
$collection -> getRelation() : array
```

---
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

---

public function index(Request $request)
اگر شما `$request` رو بدون type-hint نوشتید، لاراول فکر می‌کنه قراره مقدار از route parameter بیاد.  
مثلا `/users/{request}` → که اینجا وجود نداره.
آنگاه اگر در روت نیز هیچ پارامتری به کنترلر پاس نداده باشید آنگاه خطا خواهد داد. 