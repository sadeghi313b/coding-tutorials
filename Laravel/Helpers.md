در callback ها لاراول **تعداد آرگومان‌های callback را بررسی می‌کند**:
- اگر فقط یک آرگومان داشته باشید ← مقدار به آن پاس داده می‌شود. fn($value)
- اگر دو آرگومان داشته باشید ← مقدار و کلید به ترتیب پاس داده می‌شوند. fn(\$key,$value)
## دسترسی و بررسی داده‌ها
### بررسی وجود، قابلیت و نوع داده
<details dir="ltr"><summary  style="overflow-x: auto;white-space: nowrap;">
Arr::accessible($mixed) 
<span style="color:gray;">:bool // <bdi class="fa">بررسی دسترسی‌پذیری به‌عنوان آرایه</bdi></span>
</summary><pre style="font-size: 12px">
// Simple array
$result = Arr::accessible([1, 2, 3]); 
// true → arrays are accessible

// Collection
$result = Arr::accessible(collect([1, 2, 3])); 
// true → collections are also accessible

// String
$result = Arr::accessible("hello"); 
// false → string is not accessible as array

// Object
$result = Arr::accessible(new stdClass()); 
// false → generic objects are not accessible
</pre></details>
<details dir="ltr"><summary  style="overflow-x: auto;white-space: nowrap;">
Arr::isAssoc($array)<span style="color:gray;"> :bool <br>
<bdi class="en">Is array an associative array?</bdi>
<bdi dir="rtl" style="float: inline-end;" class="fa">بررسی اینکه آرایه associative است یا نه</bdi>
</span></summary><pre style="font-size: .7rem">
// Indexed array
$result = Arr::isAssoc([10, 20, 30]); 
// false → keys are 0,1,2 (sequential), so it's not associative

// Associative array
$result = Arr::isAssoc(['name' => 'John', 'age' => 30]); 
// true → string keys make it associative

// Mixed array
$result = Arr::isAssoc([0 => 'A', 2 => 'B']); 
// true → non-sequential numeric keys count as associative
</pre></details>
<details dir="ltr"><summary  style="overflow-x: auto;white-space: nowrap;">
Arr::isList($array) 
<span style="color:gray;">:bool // <bdi class="fa">بررسی اینکه آرایه یک list (کلیدهای متوالی عددی) است</bdi></span>
</summary><pre style="font-size: 12px">
// Indexed array with sequential keys
$result = Arr::isList([10, 20, 30]); 
// true → keys are 0,1,2, so it's a list

// Associative array
$result = Arr::isList(['name' => 'John', 'age' => 30]); 
// false → string keys make it not a list

// Mixed array with non-sequential numeric keys
$result = Arr::isList([0 => 'A', 2 => 'B']); 
// false → keys are not consecutive integers, so not a list
</pre></details>
<details dir="ltr"><summary  style="overflow-x: auto;white-space: nowrap;">
Arr::exists($array, $key) 
<span style="color:gray;">:bool // <bdi class="fa">بررسی وجود کلید مشخص در آرایه</bdi></span>
</summary><pre style="font-size: 12px">
// Simple array with numeric keys
$result = Arr::exists([10, 20, 30], 1); 
// true → key 1 exists

// Associative array
$result = Arr::exists(['name' => 'Alice', 'age' => 25], 'age'); 
// true → key 'age' exists

// Key does not exist
$result = Arr::exists(['name' => 'Alice'], 'email'); 
// false → key 'email' is missing
</pre></details>
<details dir="ltr"><summary  style="overflow-x: auto;white-space: nowrap;">
Arr::has($array, $keys) 
<span style="color:gray;">:bool // <bdi class="fa">بررسی وجود یک یا چند کلید در آرایه</bdi></span>
</summary><pre style="font-size: 12px">
// Single key exists
$result = Arr::has($userArray = ['name' => 'Alice', 'age' => 25], 'name'); 
// true → key 'name' exists

// Multiple keys exist
$result = Arr::has($userArray, ['name', 'age']); 
// true → both keys exist

// Some keys missing
$result = Arr::has($userArray, ['name', 'email']); 
// false → 'email' key does not exist

// Nested keys
$result = Arr::has($nestedArray = ['user' => ['name' => 'Alice']], 'user.name'); 
// true → nested key 'user.name' exists
</pre></details>
<details dir="ltr"><summary  style="overflow-x: auto;white-space: nowrap;">
Arr::hasAll($array, $keys) 
<span style="color:gray;">:bool // <bdi class="fa">بررسی وجود همه کلیدها در آرایه</bdi></span>
</summary><pre style="font-size: 12px">
// All keys exist
$result = Arr::hasAll($userArray = ['name' => 'Alice', 'age' => 25], ['name', 'age']); 
// true → all keys exist

// Some keys missing
$result = Arr::hasAll($userArray, ['name', 'email']); 
// false → not all keys exist
</pre></details>
<details dir="ltr"><summary  style="overflow-x: auto;white-space: nowrap;">
Arr::hasAny($array, $keys) 
<span style="color:gray;">:bool // <bdi class="fa">بررسی وجود حداقل یک کلید از کلیدهای داده شده</bdi></span>
</summary><pre style="font-size: 12px">
// At least one key exists
$result = Arr::hasAny($userArray = ['name' => 'Alice', 'age' => 25], ['name', 'email']); 
// true → 'name' exists

// None of the keys exist
$result = Arr::hasAny($userArray, ['email', 'gender']); 
// false → none of the keys exist
</pre></details>
### استخراج و بازیابی داده
<details dir="ltr"><summary  style="overflow-x: auto;white-space: nowrap;">
Arr::get($array, $key, $default = null) 
<span style="color:gray;">:mixed // <bdi class="fa">گرفتن مقدار بر اساس کلید</bdi></span>
</summary><pre style="font-size: 12px">
$userArray = ['name' => 'Alice', 'age' => 25];
$value = Arr::get($userArray, 'name'); // 'Alice' → value of 'name'
$value = Arr::get($userArray, 'email', 'unknown'); // 'unknown' → default value
</pre></details>
<details dir="ltr"><summary  style="overflow-x: auto;white-space: nowrap;">
Arr::first($array, $callback = null, $default = null) 
<span style="color:gray;">:mixed // <bdi class="fa">گرفتن اولین مقدار بر اساس شرط یا بدون شرط</bdi></span>
</summary><pre style="font-size: 12px">
$numbers = [10, 20, 30, 40];
$value = Arr::first($numbers); // 10 → first item
$value = Arr::first($numbers, fn($item) => $item > 25); // 30 → first item > 25
$value = Arr::first($numbers, fn($item) => $item > 50, 'none'); // 'none' → default
</pre></details>
<details dir="ltr"><summary  style="overflow-x: auto;white-space: nowrap;">
Arr::last($array, $callback = null, $default = null) 
<span style="color:gray;">:mixed // <bdi class="fa">گرفتن آخرین مقدار بر اساس شرط یا بدون شرط</bdi></span>
</summary><pre style="font-size: 12px">
$numbers = [10, 20, 30, 40];
$value = Arr::last($numbers); // 40 → last item
$value = Arr::last($numbers, fn($item) => $item < 35); // 30 → last item < 35
$value = Arr::last($numbers, fn($item) => $item > 50, 'none'); // 'none' → default
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::pluck($array, $valueKey, $keyKey = null) 
<span style="color:gray;">:array // <bdi class="fa">استخراج یک ستون خاص از آرایه چندبعدی</bdi></span>
</summary><pre style="font-size: 12px">
$users = [
    ['name' => 'Alice', 'age' => 25],
    ['name' => 'Bob', 'age' => 30],
    ['name' => 'Charlie', 'age' => 35],
];

// Extract a single column
$names = Arr::pluck($users, 'name'); // ['Alice', 'Bob', 'Charlie']

// Extract column with custom keys
$agesByName = Arr::pluck($users, 'age', 'name'); // ['Alice' => 25, 'Bob' => 30, 'Charlie' => 35]
</pre></details>
<details dir="ltr"><summary  style="overflow-x: auto;white-space: nowrap;">
head($array) 
<span style="color:gray;">:mixed // <bdi class="fa">گرفتن اولین عنصر از آرایه</bdi></span>
</summary><pre style="font-size: 12px">
$numbers = [10, 20, 30];
$first = head($numbers); // 10 → first element
</pre></details>
<details dir="ltr"><summary  style="overflow-x: auto;white-space: nowrap;">
Arr::sole($array, $key = null) 
<span style="color:gray;">:mixed // <bdi class="fa">گرفتن دقیقاً یک عنصر یا خطا دادن</bdi></span>
</summary><pre style="font-size: 12px">
$users = [
    ['id' => 1, 'name' => 'Alice']
];
$user = Arr::sole($users); // ['id'=>1,'name'=>'Alice']
$user = Arr::sole($users, 'id'); // 1 → value of 'id'
</pre></details>
<details dir="ltr"><summary  style="overflow-x: auto;white-space: nowrap;">
data_get($target, $key, $default = null) 
<span style="color:gray;">:mixed // <bdi class="fa">گرفتن داده از آرایه یا آبجکت با dot notation</bdi></span>
</summary><pre style="font-size: 12px">
$data = ['user' => ['name' => 'Alice', 'email' => 'alice@example.com']];
$name = data_get($data, 'user.name'); // 'Alice'
$phone = data_get($data, 'user.phone', 'unknown'); // 'unknown' → default
</pre></details>

## تغییر و به‌روزرسانی داده‌ها
### افزودن یا تغییر
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::add($array, $دnewKey, $newValue) 
<span style="color:gray;">:array // <bdi class="fa">افزودن مقدار به آرایه اگر وجود نداشت</bdi></span>
</summary><pre style="font-size: 12px">
$array = ['name' => 'Alice'];

// Add new key/value if not exists
$result = Arr::add($array, 'age', 25); // ['name' => 'Alice', 'age' => 25]

// Existing key is not overwritten
$result = Arr::add($array, 'name', 'Bob'); // ['name' => 'Alice']
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::prepend($array, $newValue, $newKey = null) 
<span style="color:gray;">:array // <bdi class="fa">افزودن مقدار به ابتدای آرایه</bdi></span>
</summary><pre style="font-size: 12px">
$array = ['b' => 2, 'c' => 3];

// Prepend value without key
$result = Arr::prepend($array, 1); // [0 => 1, 'b' => 2, 'c' => 3]

// Prepend value with key
$result = Arr::prepend($array, 1, 'a'); // ['a' => 1, 'b' => 2, 'c' => 3]
</pre></details>
<details dir="ltr"><summary  style="overflow-x: auto;white-space: nowrap;">
Arr::prependKeysWith($array, $prefix); افزودن پیشوند به کلیدها
</summary><pre style="font-size: 12px">
// Original array
$array = ['name' => 'Alice', 'age' => 25];

// Prepend 'user_' to each key
$result = Arr::prependKeysWith($array, 'user_'); // ['user_name' => 'Alice', 'user_age' => 25]

// Another example with nested keys (if supported)
$array = ['profile' => ['name' => 'Bob']];
$result = Arr::prependKeysWith($array, 'data_'); // ['data_profile' => ['name' => 'Bob']]
</pre></details>
<details dir="ltr"><summary  style="overflow-x: auto;white-space: nowrap;">
Arr::push($array, $newValue); افزودن به انتهای آرایه
</summary><pre style="font-size: 12px">
// Original array
$array = [1, 2, 3];

// Push a single value
Arr::push($array, 4); // [1, 2, 3, 4]

// Push another value
Arr::push($array, 5); // [1, 2, 3, 4, 5]

// Push multiple values using a loop or multiple calls
Arr::push($array, 6); // [1, 2, 3, 4, 5, 6]
</pre></details>
<details dir="ltr"><summary  style="overflow-x: auto;white-space: nowrap;">
Arr::set($array, $targetKey, $newValue) 
<span style="color:gray;">:void // تنظیم مقدار در کلید مشخص</span>
</summary><pre style="font-size: 12px">
// Set a value for a specific key
Arr::set($array, 'name', 'Bakhtiar'); // sets $array['name'] = 'Bakhtiar'

// Nested array using dot notation
Arr::set($array, 'user.email', 'example@mail.com'); // sets $array['user']['email'] = 'example@mail.com'
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
data_set($arrayOrObject, $targetKey, $newValue, $overwrite = true) 
<span style="color:gray;">:void // <bdi class="fa">ست کردن داده در آرایه یا آبجکت با dot notation</bdi></span>
</summary><pre style="font-size: 12px">
// Set value in a simple array
$data = ['user' => ['name' => 'Alice']];
data_set($data, 'user.age', 30); // ['user' => ['name' => 'Alice', 'age' => 30]]

// Set value in an object
$obj = (object) ['user' => (object) ['name' => 'Bob']];
data_set($obj, 'user.age', 25); // user->age is set to 25

// Using overwrite = false
$array = ['settings' => ['theme' => 'dark']];
data_set($array, 'settings.theme', 'light', false); // value remains 'dark'
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
data_fill($arrayOrObject, $targetKey, $fillValue) 
<span style="color:gray;">:void // <bdi class="fa">پر کردن کلیدهای خالی با مقدار مشخص</bdi></span>
</summary><pre style="font-size: 12px">
// Fill missing keys in an array
$data = ['user' => ['name' => 'Alice']];
data_fill($data, 'user.age', 30); // ['user' => ['name' => 'Alice', 'age' => 30]]

// Fill missing keys in a nested object
$obj = (object) ['user' => (object) ['name' => 'Bob']];
data_fill($obj, 'user.age', 25); // user->age is added as 25

// Existing keys are not overwritten
$array = ['settings' => ['theme' => 'dark']];
data_fill($array, 'settings.theme', 'light'); // value remains 'dark'
</pre></details>

### حذف
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::forget($array, $targetKey§) 
<span style="color:gray;">:void // <bdi class="fa">حذف کلید مشخص از آرایه</bdi></span>
</summary><pre style="font-size: 12px">
// Remove a simple key
$array = ['name' => 'Alice', 'age' => 25];
Arr::forget($array, 'age'); 
// Result: ['name' => 'Alice']

// Remove a nested key using dot notation
$array = ['user' => ['name' => 'Bob', 'email' => 'bob@example.com']];
Arr::forget($array, 'user.email'); 
// Result: ['user' => ['name' => 'Bob']]

// Remove multiple keys
$array = ['a' => 1, 'b' => 2, 'c' => 3];
Arr::forget($array, ['a', 'c']); 
// Result: ['b' => 2]
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::pull($array, $targetKey, $defaultValue = null) 
<span style="color:gray;">:mixed // <bdi class="fa"> برگرداندن مقدار کلید و سپس حذف آن از آرایه</bdi></span>
</summary><pre style="font-size: 12px">
// Pull an existing key
$array = ['name' => 'Alice', 'age' => 25];
$value = Arr::pull($array, 'age'); 
// $value: 25
// $array: ['name' => 'Alice']

// Pull a nested key using dot notation
$array = ['user' => ['name' => 'Bob', 'email' => 'bob@example.com']];
$value = Arr::pull($array, 'user.email'); 
// $value: "bob@example.com"
// $array: ['user' => ['name' => 'Bob']]

// Pull a non-existing key with default value
$array = ['a' => 1];
$value = Arr::pull($array, 'b', 'default'); 
// $value: "default"
// $array: ['a' => 1]
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
data_forget($target, $targetKey) 
<span style="color:gray;">:void // <bdi class="fa">حذف داده از آرایه یا آبجکت</bdi></span>
</summary><pre style="font-size: 12px">
// Forget key from simple array
$array = ['name' => 'Alice', 'age' => 25];
data_forget($array, 'age');
// $array: ['name' => 'Alice']

// Forget nested key using dot notation
$array = ['user' => ['name' => 'Bob', 'email' => 'bob@example.com']];
data_forget($array, 'user.email');
// $array: ['user' => ['name' => 'Bob']]

// Forget multiple keys
$array = ['a' => 1, 'b' => 2, 'c' => 3];
data_forget($array, ['a', 'c']);
// $array: ['b' => 2]
</pre></details>


## تغییر شکل آرایه
### ساختاردهی
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::collapse($nestedArrays)  
<span style="color:gray;">:array // <bdi class="fa">تبدیل آرایه‌های تو در تو به یک سطحی</bdi></span>
</summary><pre style="font-size: 12px">
// Collapse nested arrays into one flat array
$nested = [[1, 2], [3, 4], [5]];
$flat = Arr::collapse($nested);
// $flat: [1, 2, 3, 4, 5]

// Collapse with empty inner arrays
$nested = [[], [10, 20], []];
$flat = Arr::collapse($nested);
// $flat: [10, 20]

// Collapse array with associative subarrays
$nested = [['a' => 1], ['b' => 2]];
$flat = Arr::collapse($nested);
// $flat: ['a' => 1, 'b' => 2]
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::undot($dotNotatedArray)  
<span style="color:gray;">:array // <bdi class="fa">بازگرداندن dot notation به آرایه چندبعدی</bdi></span>
</summary><pre style="font-size: 12px">
// Convert dot-notated array into nested array
$dotNotated = [
    'user.name' => 'John',
    'user.email' => 'john@example.com',
    'settings.theme' => 'dark'
];
$nested = Arr::undot($dotNotated);
/*
$nested:
[
    'user' => [
        'name' => 'John',
        'email' => 'john@example.com',
    ],
    'settings' => [
        'theme' => 'dark',
    ]
]
*/

// Works with deeper levels
$dotNotated = [
    'company.department.team.lead' => 'Alice'
];
$nested = Arr::undot($dotNotated);
/*
$nested:
[
    'company' => [
        'department' => [
            'team' => [
                'lead' => 'Alice'
            ]
        ]
    ]
]
*/
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::dot($nestedArray, $prependKey = '')  
<span style="color:gray;">:array // <bdi class="fa">تبدیل آرایه چندبعدی به dot notation</bdi></span>
</summary><pre style="font-size: 12px">
// Convert nested array into dot-notated array
$nested = [
    'user' => [
        'name' => 'John',
        'email' => 'john@example.com',
    ],
    'settings' => [
        'theme' => 'dark',
    ]
];
$dotNotated = Arr::dot($nested);
/*
$dotNotated:
[
    'user.name' => 'John',
    'user.email' => 'john@example.com',
    'settings.theme' => 'dark'
]
*/

// Works with deeper nesting
$nested = [
    'company' => [
        'department' => [
            'team' => [
                'lead' => 'Alice'
            ]
        ]
    ]
];
$dotNotated = Arr::dot($nested);
/*
$dotNotated:
[
    'company.department.team.lead' => 'Alice'
]
*/
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::divide($array)  
<span style="color:gray;">:array // <bdi class="fa">تقسیم آرایه به کلیدها و مقادیر</bdi></span>
</summary><pre style="font-size: 12px">
// Divide array into keys and values
$array = ['name' => 'John', 'email' => 'john@example.com'];

list($keys, $values) = Arr::divide($array);

// $keys: ['name', 'email']
// $values: ['John', 'john@example.com']
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::from($value)  
<span style="color:gray;">:array // <bdi class="fa">ساخت آرایه از مقادیر مختلف</bdi></span>
</summary><pre style="font-size: 12px">
// From array
$result = Arr::from([1, 2, 3]);
// [1, 2, 3]

// From string
$result = Arr::from("hello");
// ["hello"]

// From null
$result = Arr::from(null);
// []

// From Collection
$result = Arr::from(collect(['a' => 1, 'b' => 2]));
// ['a' => 1, 'b' => 2]
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::query($array)  
<span style="color:gray;">:string // <bdi class="fa">تبدیل آرایه به query string</bdi></span>
</summary><pre style="font-size: 12px">
// Simple array
$result = Arr::query(['name' => 'John', 'age' => 25]);
// "name=John&age=25"

// Nested array
$result = Arr::query(['filters' => ['status' => 'active', 'role' => 'admin']]);
// "filters%5Bstatus%5D=active&filters%5Brole%5D=admin"

// Array with spaces
$result = Arr::query(['q' => 'hello world']);
// "q=hello+world"
</pre></details>
### نگاشت و تغییر مقدار
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::map($array, $callback)  
<span style="color:gray;">:array // <bdi class="fa">اعمال callback روی هر عنصر</bdi></span>
</summary><pre style="font-size: 12px">
// Square each number
$result = Arr::map([1, 2, 3, 4], fn($value, $key) => $value * $value);
// [1, 4, 9, 16]

// Append key to each value
$result = Arr::map(['a' => 10, 'b' => 20], fn($value, $key) => $key . '-' . $value);
// ["a" => "a-10", "b" => "b-20"]

// Convert to uppercase
$result = Arr::map(['foo', 'bar'], fn($value, $key) => strtoupper($value));
// ["FOO", "BAR"]
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::mapSpread($arrayOfArrays, $callback)  
<span style="color:gray;">:array // <bdi class="fa">اعمال callback روی آرایه‌های چندتایی</bdi></span>
</summary><pre style="font-size: 12px">
// Sum elements of sub-arrays
$result = Arr::mapSpread([[1, 2], [3, 4]], fn($a, $b) => $a + $b); 
// [3, 7]

// Concatenate elements of sub-arrays
$result = Arr::mapSpread([['foo', 'bar'], ['baz', 'qux']], fn($first, $second) => $first . '-' . $second); 
// ["foo-bar", "baz-qux"]

// Multiply elements
$result = Arr::mapSpread([[2, 3], [4, 5]], fn($x, $y) => $x * $y); 
// [6, 20]
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::mapWithKeys($array, $callback)  
<span style="color:gray;">:array // <bdi class="fa">اعمال callback و تولید کلید/مقدار جدید</bdi></span>
</summary><pre style="font-size: 12px">
// Create a new array where keys are uppercase
$result = Arr::mapWithKeys(['a' => 1, 'b' => 2], fn($value, $key) => [strtoupper($key) => $value]); 
// ["A" => 1, "B" => 2]

// Swap keys and values
$result = Arr::mapWithKeys(['first' => 'John', 'last' => 'Doe'], fn($value, $key) => [$value => $key]); 
// ["John" => "first", "Doe" => "last"]

// Prefix keys
$result = Arr::mapWithKeys(['name' => 'Alice', 'age' => 25], fn($value, $key) => ['user_' . $key => $value]); 
// ["user_name" => "Alice", "user_age" => 25]
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::reject($array, $callback)  
<span style="color:gray;">:array // <bdi class="fa">حذف مقادیر بر اساس شرط</bdi></span>
</summary><pre style="font-size: 12px">
// Remove even numbers
$result = Arr::reject([1, 2, 3, 4, 5], fn($value) => $value % 2 === 0); 
// [1, 3, 5]

// Remove empty strings
$result = Arr::reject(['apple', '', 'banana'], fn($value) => $value === ''); 
// ["apple", "banana"]

// Remove values greater than 10
$result = Arr::reject([5, 12, 8, 20], fn($value) => $value > 10); 
// [5, 8]
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::select($array, $callback)  
<span style="color:gray;">:array // <bdi class="fa">انتخاب مقادیر بر اساس شرط</bdi></span>
</summary><pre style="font-size: 12px">
// Select even numbers
$result = Arr::select([1, 2, 3, 4, 5], fn($value) => $value % 2 === 0); 
// [2, 4]

// Select non-empty strings
$result = Arr::select(['apple', '', 'banana'], fn($value) => $value !== ''); 
// ["apple", "banana"]

// Select values less than 10
$result = Arr::select([5, 12, 8, 20], fn($value) => $value &lt; 10); 
// [5, 8]
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::where($array, $callback)  
<span style="color:gray;">:array // <bdi class="fa">فیلتر عناصر بر اساس شرط</bdi></span>
</summary><pre style="font-size: 12px">
// Keep only even numbers
$result = Arr::where([1, 2, 3, 4, 5], fn($value) => $value % 2 === 0); 
// [2, 4]

// Keep strings with length > 3
$result = Arr::where(['cat', 'lion', 'tiger'], fn($value) => strlen($value) > 3); 
// ["lion", "tiger"]

// Keep values greater than 10
$result = Arr::where([5, 12, 8, 20], fn($value) => $value > 10); 
// [12, 20]
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::whereNotNull($array, $targetKey)  
<span style="color:gray;">:array // <bdi class="fa">انتخاب عناصری که فلان کلید وجود دارد و null نیست</bdi></span>
</summary><pre style="font-size: 12px">
// Keep elements where value is not null
$result = Arr::whereNotNull(['name' => 'Alice', 'age' => null, 'city' => 'Paris'], 'age'); 
// ['age' => null] excluded, remaining ['name' => 'Alice', 'city' => 'Paris']

// Keep elements where key 'email' exists and is not null
$result = Arr::whereNotNull([
    ['email' => 'a@example.com'], 
    ['email' => null], 
    ['name' => 'Bob']
], 'email');
// [['email' => 'a@example.com']]
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::only($array, $key§)  
<span style="color:gray;">:array // <bdi class="fa">انتخاب کلیدهای مشخص و برگرداندن کلید و مقدار</bdi></span>
</summary><pre style="font-size: 12px">
// Select only the specified keys from the array
$array = ['name' => 'Alice', 'age' => 25, 'city' => 'Paris'];
$result = Arr::only($array, ['name', 'city']); 
// ['name' => 'Alice', 'city' => 'Paris']

// Selecting a single key
$result = Arr::only($array, ['age']); 
// ['age' => 25]
</pre></details>

## مرتب‌سازی و سازماندهی
### مرتب‌سازی
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::sort($array, $callback = null)  
<span style="color:gray;">:array // <bdi class="fa">مرتب‌سازی بر اساس مقدار</bdi></span>
</summary><pre style="font-size: 12px">
// Sort a simple numeric array
$array = [5, 3, 8, 1];
$result = Arr::sort($array); 
// [1, 3, 5, 8]

// Sort an array using a callback
$array = ['apple' => 3, 'banana' => 1, 'cherry' => 2];
$result = Arr::sort($array, fn($value) => $value); 
// ['banana' => 1, 'cherry' => 2, 'apple' => 3]
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::sortDesc($array, $callback = null)  
<span style="color:gray;">:array // <bdi class="fa">مرتب‌سازی نزولی بر اساس مقدار</bdi></span>
</summary><pre style="font-size: 12px">
// Sort a numeric array in descending order
$array = [5, 3, 8, 1];
$result = Arr::sortDesc($array); 
// [8, 5, 3, 1]

// Sort an associative array in descending order using a callback
$array = ['apple' => 3, 'banana' => 1, 'cherry' => 2];
$result = Arr::sortDesc($array, fn($value) => $value); 
// ['apple' => 3, 'cherry' => 2, 'banana' => 1]
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::sortRecursive($array)  
<span style="color:gray;">:array // <bdi class="fa">مرتب‌سازی بازگشتی آرایه و زیرآرایه‌ها</bdi></span>
</summary><pre style="font-size: 12px">
// Sort a multidimensional array recursively
$array = [
    'fruits' => ['banana', 'apple', 'cherry'],
    'numbers' => [3, 1, 2]
];
$result = Arr::sortRecursive($array); 
// [
//     'fruits' => ['apple', 'banana', 'cherry'],
//     'numbers' => [1, 2, 3]
// ]
</pre></details>
### گروه‌بندی و دسته‌بندی
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::keyBy($array, $callback)  
<span style="color:gray;">:array // <bdi class="fa">تولید آرایه با کلیدهای جدید بر اساس callback</bdi></span>
</summary><pre style="font-size: 12px">
// Key array by a field name
$array = [
    ['id' => 1, 'name' => 'Alice'],
    ['id' => 2, 'name' => 'Bob']
];
$result = Arr::keyBy($array, 'id'); 
// [
//     1 => ['id' => 1, 'name' => 'Alice'],
//     2 => ['id' => 2, 'name' => 'Bob']
// ]

// Key array by a callback
$result = Arr::keyBy($array, function($item) {
    return strtoupper($item['name']);
});
// [
//     'ALICE' => ['id' => 1, 'name' => 'Alice'],
//     'BOB' => ['id' => 2, 'name' => 'Bob']
// ]
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::keyBy($array, $callback)  
<span style="color:gray;">:array // <bdi class="fa">تولید آرایه با کلیدهای جدید بر اساس callback</bdi></span>
</summary><pre style="font-size: 12px">
// Key array by a field name
$array = [
    ['id' => 1, 'name' => 'Alice'],
    ['id' => 2, 'name' => 'Bob']
];
$result = Arr::keyBy($array, 'id'); 
// [
//     1 => ['id' => 1, 'name' => 'Alice'],
//     2 => ['id' => 2, 'name' => 'Bob']
// ]

// Key array by a callback
$result = Arr::keyBy($array, function($item) {
    return strtoupper($item['name']);
});
// [
//     'ALICE' => ['id' => 1, 'name' => 'Alice'],
//     'BOB' => ['id' => 2, 'name' => 'Bob']
// ]
</pre></details>

## عملیات ریاضی و آماری
### انتخاب و تصادفی
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::random($array, $howManyElements = null)  
<span style="color:gray;">:mixed|array // <bdi class="fa">انتخاب یک یا چند عنصر به‌صورت تصادفی از آرایه</bdi></span>
</summary><pre style="font-size: 12px">
// Select a single random item
$array = [1, 2, 3, 4, 5];
$item = Arr::random($array); // e.g., 3

// Select multiple random items
$items = Arr::random($array, 2); // e.g., [2, 5]
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::take($array, $maxNumber)  
<span style="color:gray;">:array // <bdi class="fa">گرفتن تعداد مشخصی از عناصر ابتدای آرایه</bdi></span>
</summary><pre style="font-size: 12px">
// Take first 3 items
$array = [1, 2, 3, 4, 5];
$subset = Arr::take($array, 3); // [1, 2, 3]

// Take more items than exist
$subset = Arr::take($array, 10); // [1, 2, 3, 4, 5]
</pre></details>
### اعتبارسنجی و شرطی
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::every($array, $callback)  
<span style="color:gray;">:bool // <bdi class="fa">بررسی اینکه آیا تمام عناصر آرایه شرط مشخص شده را دارند</bdi></span>
</summary><pre style="font-size: 12px">
// Check if all numbers are positive
$array = [2, 4, 6];
$result = Arr::every($array, fn($value) => $value > 0); // true

// Check if all numbers are even
$array = [2, 3, 4];
$result = Arr::every($array, fn($value) => $value % 2 === 0); // false
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Arr::some($array, $callback)  
<span style="color:gray;">:bool // <bdi class="fa">بررسی اینکه آیا حداقل یک عنصر آرایه شرط مشخص شده را دارد</bdi></span>
</summary><pre style="font-size: 12px">
// Check if at least one number is even
$array = [1, 3, 4];
$result = Arr::some($array, fn($value) => $value % 2 === 0); // true

// Check if at least one number is negative
$array = [1, 3, 5];
$result = Arr::some($array, fn($value) => $value &lt; 0); // false
</pre></details>

## ترکیب و اتصال
### اتصال
<details dir="ltr"><summary  style="overflow-x: auto;white-space: nowrap;">
Arr::join($array, $glue = '', $finalGlue = '')
<span style="color:gray;">:string // <bdi class="fa">اتصال مقادیر آرایه با جداکننده</bdi></span>
</summary><pre style="font-size: 12px">
// Simple join
$array = ['apple', 'banana', 'cherry'];
$result = Arr::join($array, ', '); // "apple, banana, cherry"

// Join with final glue for the last element
$array = ['red', 'green', 'blue'];
$result = Arr::join($array, ', ', ' & '); // "red, green & blue"

// Join array of numbers
$numbers = [1, 2, 3, 4];
$result = Arr::join($numbers, '-'); // "1-2-3-4"
</pre></details>
### ضربدری
<details dir="ltr"><summary  style="overflow-x: auto;white-space: nowrap;">
Arr::crossJoin($arrays)
<span style="color:gray;">:array // <bdi class="fa">ضرب دکارتی آرایه‌ها</bdi></span>
</summary><pre style="font-size: 12px">
// Cross join of two arrays
$array1 = [1, 2];
$array2 = ['a', 'b'];
$result = Arr::crossJoin($array1, $array2);
// [
//     [1, 'a'],
//     [1, 'b'],
//     [2, 'a'],
//     [2, 'b'],
// ]

// Cross join of three arrays
$array3 = ['X', 'Y'];
$result = Arr::crossJoin($array1, $array2, $array3);
// [
//     [1, 'a', 'X'], [1, 'a', 'Y'],
//     [1, 'b', 'X'], [1, 'b', 'Y'],
//     [2, 'a', 'X'], [2, 'a', 'Y'],
//     [2, 'b', 'X'], [2, 'b', 'Y'],
// ]
</pre></details>
## خروجی و نمایش
### تبدیل و نمایش
<details dir="ltr"><summary  style="overflow-x: auto;white-space: nowrap;">
Arr::toCssClasses($arrayOfClasses)
<span style="color:gray;">:string // <bdi class="fa">تولید رشته کلاس‌های CSS از آرایه</bdi></span>
</summary><pre style="font-size: 12px">
// Simple array of classes
$classes = ['btn', 'btn-primary', 'active'];
$result = Arr::toCssClasses($classes); // "btn btn-primary active"

// Array with conditional classes
$classes = [
    'btn',
    'btn-primary' => true,
    'disabled' => false,
];
$result = Arr::toCssClasses($classes); // "btn btn-primary"

// Dynamic evaluation with callback
$classes = [
    'btn',
    'btn-large' => fn() => true,
    'hidden' => fn() => false,
];
$result = Arr::toCssClasses($classes); // "btn btn-large"
</pre></details>
<details dir="ltr"><summary  style="overflow-x: auto;white-space: nowrap;">
Arr::toCssStyles($styles)
<span style="color:gray;">:string // <bdi class="fa">تولید استایل CSS از آرایه</bdi></span>
</summary><pre style="font-size: 12px">
// Convert associative array to CSS string
$styles = [
    'color' => 'red',
    'font-size' => '14px',
    'margin' => '10px'
];
$result = Arr::toCssStyles($styles);
// "color: red; font-size: 14px; margin: 10px;"

// Works with empty array
$result = Arr::toCssStyles([]);
// ""
</pre></details>
## کمکی (Utility)
### تبدیل نوع
<details dir="ltr"><summary  style="overflow-x: auto;white-space: nowrap;">
Arr::array($value)
<span style="color:gray;">:array // <bdi class="fa">تبدیل به آرایه</bdi></span>
</summary><pre style="font-size: 12px">
// Convert collection to array
$collection = collect([1, 2, 3]);
$result = Arr::array($collection); // [1, 2, 3]
<br>// Convert null to empty array
$result = Arr::array(null); // []
<br>// Convert scalar to array
$result = Arr::array(42); // [42]
</pre></details>
<details dir="ltr"><summary  style="overflow-x: auto;white-space: nowrap;">
Arr::boolean($value)
<span style="color:gray;">:bool // <bdi class="fa">تبدیل به بولین</bdi></span>
</summary><pre style="font-size: 12px">
// Integer to boolean
$result = Arr::boolean(1); // true
$result = Arr::boolean(0); // false

// String to boolean
$result = Arr::boolean("true");  // true
$result = Arr::boolean("false"); // false

// Null to boolean
$result = Arr::boolean(null); // false
</pre></details>
<details dir="ltr"><summary  style="overflow-x: auto;white-space: nowrap;">
Arr::float($value)
<span style="color:gray;">:float // <bdi class="fa">تبدیل به عدد اعشاری</bdi></span>
</summary><pre style="font-size: 12px">
// Integer to float
$result = Arr::float(10); // 10.0

// String to float
$result = Arr::float("12.34"); // 12.34

// Boolean to float
$result = Arr::float(true); // 1.0
$result = Arr::float(false); // 0.0
</pre></details>
<details dir="ltr"><summary  style="overflow-x: auto;white-space: nowrap;">
Arr::integer($value)
<span style="color:gray;">:int // <bdi class="fa">تبدیل به عدد صحیح</bdi></span>
</summary><pre style="font-size: 12px">
// Float to integer
$result = Arr::integer(12.34); // 12

// String to integer
$result = Arr::integer("56"); // 56

// Boolean to integer
$result = Arr::integer(true); // 1
$result = Arr::integer(false); // 0
</pre></details>
<details dir="ltr"><summary  style="overflow-x: auto;white-space: nowrap;">
Arr::string($value)
<span style="color:gray;">:string // <bdi class="fa">تبدیل به رشته</bdi></span>
</summary><pre style="font-size: 12px">
// Integer to string
$result = Arr::string(123); // "123"

// Float to string
$result = Arr::string(45.67); // "45.67"

// Boolean to string
$result = Arr::string(true); // "1"
$result = Arr::string(false); // ""

// Array to string
$result = Arr::string([1, 2, 3]); // "Array"
</pre></details>
### بسته‌بندی
<details dir="ltr"><summary  style="overflow-x: auto;white-space: nowrap;">
Arr::wrap($value)
<span style="color:gray;">:array // <bdi class="fa">تبدیل مقدار به آرایه اگر آرایه نباشد</bdi></span>
</summary><pre style="font-size: 12px">
// Single value
$result = Arr::wrap(123); // [123]

// Null value
$result = Arr::wrap(null); // []

// Already an array
$result = Arr::wrap([1, 2, 3]); // [1, 2, 3]

// Collection
$result = Arr::wrap(collect([1, 2])); // [1, 2]
</pre></details>
# URL
## URL & Routing Helpers
### Route Generation
action($action, $parameters = [], $absolute = true)  
<span style="color:gray;" class="en">Generates a URL for a controller action using the given action and parameters</span>
to_action($action, $parameters = [], $absolute = true)  
<span style="color:gray;" class="en">Alias of action(), generates a URL to a controller action</span>
to_route($name, $parameters = [], $absolute = true)  
<span style="color:gray;" class="en">Generates a URL for a named route by its route name</span>
route($name, $parameters = [], $absolute = true)  
<span style="color:gray;" class="en">Generates a full URL to a named route</span>
### URL & Asset Generation
url($path = null, $parameters = [], $secure = null)  
<span style="color:gray;" class="en">Generates a full URL for the specified path with optional parameters</span>
uri($path = null)  
<span style="color:gray;" class="en">Retrieves the current request URI or appends a given path</span>
asset($path, $secure = null)  
<span style="color:gray;" class="en">Generates a URL to an asset located in the public directory</span>
secure_asset($path)  
<span style="color:gray;" class="en">Generates a secure HTTPS URL to a public asset</span>
secure_url($path)  
<span style="color:gray;" class="en">Generates a secure HTTPS URL to a specified path</span>