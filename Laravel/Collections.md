## Item Access / Retrieval: دسترسی یا بازیابی آیتم‌ها از مجموعه
### Single Item Retrieval: بازیابی یک آیتم مشخص یا شرطی
<details dir="ltr"><summary>
->after($someItem); آیتم بعد از مقدار مشخص شده را برمی‌گرداند
</summary><pre style="font-size: 12px">
$collection->after(
    $someItem,       // The value or callback to locate the reference item
    $strict = false  // Whether to use strict comparison (===)
);

// Example 1: basic usage
collect([1, 2, 3, 4])->after(2); // returns 3

// Example 2: strict comparison
collect([2, 4, 6, 8])->after('4', strict: true); // returns null (because '4' !== 4)

// Example 3: using a callback
collect([2, 4, 6, 8])->after(function (int $item, int $key) {
    return $item > 5;
}); // returns 8

// Example 4: if item not found
collect([10, 20, 30])->after(50); // returns null
</pre></details>
<details dir="ltr"><summary >
->before($someItem); آیتم قبل از مقدار مشخص شده را برمی‌گرداند
</summary><pre style="font-size: 12px">
$collection->before(
    $someItem,       // The value or callback to locate the reference item
    $strict = false  // Whether to use strict comparison (===)
);

// Example 1: basic usage
collect([1, 2, 3, 4])->before(3); // returns 2

// Example 2: strict comparison
collect([2, 4, 6, 8])->before('6', strict: true); // returns null (because '6' !== 6)

// Example 3: using a callback
collect([2, 4, 6, 8])->before(function (int $item, int $key) {
    return $item > 5;
}); // returns 4

// Example 4: if item not found
collect([10, 20, 30])->before(5); // returns null
</pre></details>
<details dir="ltr"><summary>
->first($callback = null); اولین آیتم مجموعه را برمی‌گرداند، با امکان شرط دلخواه
</summary><pre style="font-size: 12px">
$collection->first(
    $callback = null,  // Optional callback to filter items
    $default = null    // Value to return if no items match
);

// Example 1: basic usage
collect([1, 2, 3, 4])->first(); // returns 1

// Example 2: using a callback
collect([1, 2, 3, 4])->first(function ($item) {
    return $item > 2;
}); // returns 3

// Example 3: callback with default value
collect([1, 2, 3, 4])->first(function ($item) {
    return $item > 10;
}, 'not found'); // returns 'not found'
</pre></details>
<details dir="ltr"><summary>
->firstOrFail(); اولین آیتم را برمی‌گرداند و در صورت نبود خطا ایجاد می‌کند
</summary><pre style="font-size: 12px">
$collection->firstOrFail(
    $callback = null  // Optional callback to filter items
);

collect([1, 2, 3])->firstOrFail(); // returns 1
collect([1, 2, 3])->firstOrFail(function ($item) { return $item > 1; }); // returns 2
collect([])->firstOrFail(); // throws Illuminate\Support\ItemNotFoundException
</pre></details>
<details dir="ltr"><summary>
->firstWhere($key, $value, $operator = null); اولین آیتم مطابق کلید/مقدار مشخص شده را برمی‌گرداند
</summary><pre style="font-size: 12px">
$collection->firstWhere(
    $key,       // Field or key to check
    $value,     // Value to compare
    $operator = '='  // Comparison operator: '===', '!==', '!=', '==', '=', etc
);

collect([['price'=>100], ['price'=>200]])->firstWhere('price', 200); // returns ['price'=>200]
collect([['price'=>100], ['price'=>200]])->firstWhere('price', 150, '>'); // returns ['price'=>200]
collect([['price'=>100]])->firstWhere('price', 200); // returns null
</pre></details>
<details dir="ltr"><summary>
->last(); آخرین آیتم مجموعه را برمی‌گرداند
</summary><pre style="font-size: 12px">
$collection->last(
    $callback = null,  // Optional callback to filter items
    $default = null    // Value to return if no items match
);

collect([1,2,3])->last(); // returns 3
collect([1,2,3,4])->last(function ($item) { return $item % 2 === 0; }); // returns 4
collect([1,3,5])->last(function ($item) { return $item % 2 === 0; }, 'not found'); // returns 'not found'
</pre></details>
<details dir="ltr"><summary>
->sole($keyOrCallback = null); یک آیتم مشخص مطابق شرط یا کلید را برمی‌گرداند
</summary><pre style="font-size: 12px">
$collection->sole(
    $keyOrCallback = null  // Key or callback to locate a single item
);

collect([1])->sole(); // returns 1
collect([1,2,3])->sole(function ($item) { return $item > 2; }); // returns 3
collect([1,2,3,4])->sole(function ($item) { return $item > 1; }); // throws Illuminate\Support\MultipleItemsFoundException
</pre></details>

### Multiple Item Retrieval: بازیابی چند آیتم یا کل مجموعه
<details dir="ltr"><summary>
->all(); تمامی آیتم‌های مجموعه را به صورت آرایه برمی‌گرداند
</summary><pre style="font-size: 12px">
$collection->all();

collect([1,2,3])->all(); // returns [1,2,3]
collect([])->all(); // returns []
</pre></details>
<details dir="ltr"><summary>
->get($key, $defaultValue = null); مقدار آیتم با کلید مشخص را برمی‌گرداند، مقدار پیش‌فرض اختیاری
</summary><pre style="font-size: 12px">
$collection->get(
    $key,           // The key of the item to retrieve
    $defaultValue = null  // Value to return if key does not exist
);

collect(['name'=>'Alice', 'age'=>25])->get('name'); // returns 'Alice'
collect(['name'=>'Alice'])->get('age', 0); // returns 0
</pre></details>
<details dir="ltr"><summary>
->slice($offset, $length = null); زیرمجموعه‌ای از آیتم‌ها بر اساس شاخص برمی‌گرداند
</summary><pre style="font-size: 12px">
$collection->slice(
    $offset,      // Starting index
    $length = null // Number of items to return (all remaining if null)
);

collect([1,2,3,4,5])->slice(2); // returns [3,4,5]
collect([1,2,3,4,5])->slice(1, 3); // returns [2,3,4]
</pre></details>
<details dir="ltr"><summary>
->forPage($pageNumber, $perPage); آیتم‌های مربوط به یک صفحه مشخص را برمی‌گرداند
</summary><pre style="font-size: 12px">
$collection->forPage(
    $pageNumber, // Page number (1-based)
    $perPage     // Number of items per page
);

collect([1,2,3,4,5,6])->forPage(2, 2); // returns [3,4]
collect([1,2,3,4,5,6])->forPage(3, 2); // returns [5,6]
</pre></details>
## Filtering / Conditionals: متدهایی برای فیلتر کردن یا اعمال شرایط
### Basic Filtering: فیلتر ساده با callback یا کلید/مقدار
<details dir="ltr"><summary>
->filter($callback = null); <bdi>آیتم‌ها را با callback فیلتر می‌کند</bdi>
</summary><pre style="font-size: 12px">
$collection->filter(
    $callback = null // Callback function to determine if item should be included
);

collect([1,2,3,4,5])->filter(fn($item) => $item > 2); // returns [3,4,5]
collect([1,2,3,4,5])->filter(); // returns all non-falsy items
</pre></details>
<details dir="ltr"><summary>
->reject($callback); <bdi> آیتم‌ها را با callback حذف می‌کند</bdi>
</summary><pre style="font-size: 12px">
$collection->reject(
    $callback // Callback function to determine if item should be excluded
);

collect([1,2,3,4,5])->reject(fn($item) => $item > 3); // returns [1,2,3]
</pre></details>
<details dir="ltr"><summary>
->where($key, $operator = null, $value = null); فیلتر آیتم‌ها با کلید/مقدار مشخص
</summary><pre style="font-size: 12px">
$collection->where(
    $key,        // Field/key to check
    $operator = null, // Comparison operator: '===', '!==', '!=', '==', '=', '&lt;&gt;', '&gt;', '&lt;', '>=', '&lt;='
    $value = null     // Value to compare against
);

collect([
    ['product'=>'Desk','price'=>200],
    ['product'=>'Chair','price'=>100]
])->where('price', '=', 100); // returns [['product'=>'Chair','price'=>100]]

collect([2,4,6,8])->where('value', '&gt;', 5); // returns [6,8]
collect([2,4,6,8])->where('value', '&lt;', 5); // returns [2,4]
</pre></details>
<details dir="ltr"><summary>
-> whereStrict($key, $value); فیلتر با مقایسه سخت
</summary><pre style="font-size: 12px">
$collection->whereStrict(
    $key,                 // کلید یا نام فیلد برای مقایسه
    $value                // مقدار مورد مقایسه با مقایسه سخت (strict)
);
$filtered = $collection->whereStrict('price', 100); // آیتم‌هایی که قیمت آنها دقیقاً برابر 100 است
$filtered = $collection->whereStrict('name', 'John'); // آیتم‌هایی که نام آنها دقیقاً 'John' است
</pre></details>
<details dir="ltr"><summary>
-> whereBetween($key, $range); آیتم‌ها بین دو مقدار مشخص
</summary><pre style="font-size: 12px">
$collection->whereBetween(
    $key,     // کلید یا نام فیلد برای مقایسه
    $range    // آرایه شامل مقدار حداقل و حداکثر [min, max]
);
$filtered = $collection->whereBetween('price', [50, 150]); // آیتم‌هایی که قیمتشان بین 50 و 150 است
$filtered = $collection->whereBetween('age', [18, 30]);    // آیتم‌هایی که سنشان بین 18 و 30 است
</pre></details>
<details dir="ltr"><summary>
-> whereNotBetween($key, $range); آیتم‌ها خارج از بازه مشخص
</summary><pre style="font-size: 12px">
$collection->whereNotBetween(
    $key,    // کلید یا نام فیلد برای مقایسه
    $range   // آرایه شامل مقدار حداقل و حداکثر [min, max]
);
$filtered = $collection->whereNotBetween('price', [50, 150]); // آیتم‌هایی که قیمتشان خارج از 50 تا 150 است
</pre></details>
<details dir="ltr"><summary>
-> whereIn($key, $values); آیتم‌ها در آرایه مشخص
</summary><pre style="font-size: 12px">
$collection->whereIn(
    $key,     // کلید یا نام فیلد
    $values   // آرایه مقادیر معتبر
);
$filtered = $collection->whereIn('category', ['Books', 'Electronics']); // آیتم‌هایی که دسته‌شان یکی از این مقادیر است
</pre></details>
<details dir="ltr"><summary>
-> whereInStrict($key, $values); آیتم‌ها در آرایه با مقایسه سخت
</summary><pre style="font-size: 12px">
$collection->whereInStrict(
    $key,     // کلید یا نام فیلد
    $values   // آرایه مقادیر معتبر با مقایسه سخت
);
$filtered = $collection->whereInStrict('category', ['Books', 'Electronics']); // آیتم‌هایی که دسته‌شان دقیقاً یکی از این مقادیر است
</pre></details>
<details dir="ltr"><summary>
-> whereNotIn($key, $values); آیتم‌ها خارج از آرایه مشخص
</summary><pre style="font-size: 12px">
$collection->whereNotIn(
    $key,     // کلید یا نام فیلد
    $values   // آرایه مقادیر برای حذف
);
$filtered = $collection->whereNotIn('category', ['Books', 'Electronics']); // آیتم‌هایی که دسته‌شان یکی از این مقادیر نیست
</pre></details>
<details dir="ltr"><summary>
-> whereNotInStrict($key, $values); آیتم‌ها خارج از آرایه با مقایسه سخت
</summary><pre style="font-size: 12px">
$collection->whereNotInStrict(
    $key,     // کلید یا نام فیلد
    $values   // آرایه مقادیر برای حذف با مقایسه سخت
);
$filtered = $collection->whereNotInStrict('category', ['Books', 'Electronics']); // آیتم‌هایی که دسته‌شان دقیقاً یکی از این مقادیر نیست
</pre></details>
<details dir="ltr"><summary>
-> whereNotNull($key); <span dir="rtl">آیتم‌هایی که مقدار کلید null نیست</span>
</summary><pre style="font-size: 12px">
$collection->whereNotNull(
    $key  // کلید یا نام فیلد برای بررسی مقدار غیر null
);
$filtered = $collection->whereNotNull('deleted_at'); // آیتم‌هایی که deleted_at آن‌ها null نیست
</pre></details>
<details dir="ltr"><summary>
-> whereNotNull($key); <span dir="rtl">آیتم‌هایی که مقدار کلید null نیست</span>
</summary><pre style="font-size: 12px">
$collection->whereNotNull(
    $key  // کلید یا نام فیلد برای بررسی مقدار غیر null
);
$filtered = $collection->whereNotNull('deleted_at'); // آیتم‌هایی که deleted_at آن‌ها null نیست
</pre></details>
### Conditional Execution: اجرای callback مشروط
<details dir="ltr"><summary>
-> when($condition, $callback); <span dir=rtl>callback را وقتی شرط درست باشد اجرا می‌کند</span>

</summary><pre style="font-size: 12px">
$collection->when(
    $condition,  // شرطی که در صورت true بودن callback اجرا می‌شود
    $callback    // callback برای اجرای عملیاتی روی مجموعه
);
$collection->when($userIsAdmin, fn($c) => $c->push('admin')); // وقتی کاربر ادمین است، آیتم admin اضافه می‌شود
</pre></details>
<details dir="ltr"><summary>
-> whenEmpty($callback); <span dir=rtl>callback وقتی مجموعه خالی باشد اجرا می‌شود</span>

</summary><pre style="font-size: 12px">
$collection->whenEmpty(
    $callback  // callback برای اجرای عملیاتی وقتی مجموعه خالی است
);
$collection->whenEmpty(fn($c) => $c->push('default')); // اگر مجموعه خالی بود، آیتم default اضافه می‌شود
</pre></details>
<details dir="ltr"><summary>
-> whenNotEmpty($callback); <span dir=rtl>callback وقتی مجموعه خالی نیست اجرا می‌شود</span>

</summary><pre style="font-size: 12px">
$collection->whenNotEmpty(
    $callback  // Callback to execute when the collection is not empty
);
$collection->whenNotEmpty(fn($c) => $c->push('exists')); // Adds 'exists' if collection is not empty
</pre></details>
<details dir="ltr"><summary>
-> unless($condition, $callback); <span dir=rtl>callback وقتی شرط غلط باشد اجرا می‌شود</span>

</summary><pre style="font-size: 12px">
$collection->unless(
    $condition, // Condition; callback runs if this is false
    $callback   // Callback to execute when condition is false
);
$collection->unless($userIsAdmin, fn($c) => $c->push('guest')); // Adds 'guest' if user is not admin
</pre></details>
<details dir="ltr"><summary>
-> unlessEmpty($callback); <span dir=rtl>callback وقتی مجموعه خالی نیست اجرا می‌شود</span>

</summary><pre style="font-size: 12px">
$collection->unlessEmpty(
    $callback // Callback to execute when the collection is not empty
);
$collection->unlessEmpty(fn($c) => $c->push('exists')); // Adds 'exists' if collection is not empty
</pre></details>
<details dir="ltr"><summary>
-> unlessNotEmpty($callback); <span dir=rtl>callback وقتی مجموعه خالی باشد اجرا می‌شود</span>

</summary><pre style="font-size: 12px">
$collection->unlessNotEmpty(
    $callback // Callback to execute when the collection is empty
);
$collection->unlessNotEmpty(fn($c) => $c->push('default')); // Adds 'default' if collection is empty
</pre></details>
## Transformation / Mapping: تغییر یا تبدیل آیتم‌ها
### Mapping: اعمال callback یا تبدیل به کلاس یا گروه‌بندی
<details dir="ltr"><summary>
-> map($callback); <span dir=rtl>callback را روی هر آیتم اعمال می‌کند</span>

</summary><pre style="font-size: 12px">
$collection->map(
    $callback // Callback to apply to each item
);
$collection->map(fn($item) => $item * 2); // Doubles each item
</pre></details>
<details dir="ltr"><summary>
-> mapInto($className); هر آیتم را به کلاس مشخص تبدیل می‌کند
</summary><pre style="font-size: 12px">
$collection->mapInto(
    $className // Class name to convert each item into
);
$collection->mapInto(User::class); // Converts each item to a User instance
</pre></details>
<details dir="ltr"><summary>
-> mapToGroups($callback); آیتم‌ها را گروه‌بندی می‌کند
</summary><pre style="font-size: 12px">
$collection->mapToGroups(
    $callback // Callback returning [groupKey => value] for grouping
);
$collection->mapToGroups(fn($item) => [$item->type => $item]); // Groups items by type
</pre></details>
<details dir="ltr"><summary>
-> mapWithKeys($callback); هر آیتم را به یک جفت کلید/مقدار تبدیل می‌کند
</summary><pre style="font-size: 12px">
$collection->mapWithKeys(
    $callback // Callback returning [key => value] for each item
);
$collection->mapWithKeys(fn($item) => [$item->id => $item->name]); // Maps id to name
</pre></details>
<details dir="ltr"><summary>
-> mapSpread($callback);<span dir=rtl> callback روی آیتم‌های چندبعدی اعمال می‌شود</span>
</summary><pre style="font-size: 12px">
$collection->mapSpread(
    $callback // Callback to apply to each sub-array spread as arguments
);
$collection->mapSpread(fn($a, $b) => $a + $b); // Sums pairs of values
</pre></details>
<details dir="ltr"><summary>
-> transform($callback); مجموعه را در محل تغییر می‌دهد
</summary><pre style="font-size: 12px">
$collection->transform(
    $callback // Callback to transform each item in-place
);
$collection->transform(fn($item) => $item * 2); // Doubles each item in the same collection
</pre></details>
<details dir="ltr"><summary>
-> flatMap($callback); هر آیتم را به مجموعه جدیدی نگاشت می‌کند
</summary><pre style="font-size: 12px">
$collection->flatMap(
    $callback // Callback that returns a collection for each item
);
$collection->flatMap(fn($item) => collect([$item, $item * 2])); // Maps each item to a collection of itself and its double
</pre></details>
 
## Grouping / Chunking: تقسیم یا گروه‌بندی مجموعه
### Chunking: تقسیم به بخش‌های کوچکتر
<details dir="ltr"><summary>
-> chunk($size); تقسیم مجموعه به قطعات با اندازه مشخص
</summary><pre style="font-size: 12px">
$collection->chunk(
    $size // Number of items per chunk
);
$collection->chunk(2); // Splits collection into chunks of 2 items each
</pre></details>
<details dir="ltr"><summary>
-> chunkWhile($callback); تقسیم مجموعه تا وقتی شرط برقرار است
</summary><pre style="font-size: 12px">
$collection->chunkWhile(
    $callback // Callback returning true to continue chunking
);
$collection->chunkWhile(fn($value, $key) => $value < 5); // Chunks while value < 5
</pre></details>
<details dir="ltr"><summary>
-> split($numberOfGroups); تقسیم مجموعه به تعداد مشخص
</summary><pre style="font-size: 12px">
$collection->split(
    $numberOfGroups // Number of groups to split into
);
$collection->split(3); // Splits collection into 3 groups
</pre></details>
<details dir="ltr"><summary>
-> splitIn($size); تقسیم مجموعه به اندازه مشخص
</summary><pre style="font-size: 12px">
$collection->splitIn(
    $size // Size of each group
);
$collection->splitIn(2); // Splits collection into groups of 2 items
</pre></details>
<details dir="ltr"><summary>
-> sliding($windowSize, $step = 1); ایجاد پنجره‌های متحرک از مجموعه
</summary><pre style="font-size: 12px">
$collection->sliding(
    $windowSize, // Number of items in each window
    $step = 1    // Step size for sliding window
);
$collection->sliding(2, 1); // Creates sliding windows of 2 items with step 1
</pre></details>
<details dir="ltr"><summary>
-> partition($callback); <span dir=rtl>تقسیم مجموعه بر اساس callback</span>

</summary><pre style="font-size: 12px">
$collection->partition(
    $callback // Callback returning true/false to partition items
);
$collection->partition(fn($item) => $item > 5); // Splits items into those >5 and &lt;=5
</pre></details>

### Grouping: گروه‌بندی بر اساس کلید یا callback
<details dir="ltr"><summary>
-> groupBy($keyOrCallback); <span dir=rtl>گروه‌بندی آیتم‌ها بر اساس کلید یا callback</span>

</summary><pre style="font-size: 12px">
$collection->groupBy(
    $keyOrCallback // Key name or callback function to group by
);
$collection->groupBy('category'); // Groups items by 'category' key
$collection->groupBy(fn($item) => $item->type); // Groups items using callback
</pre></details>
<details dir="ltr"><summary>
-> keyBy($keyOrCallback); <span dir=rtl>کلیدهای مجموعه را بر اساس callback تغییر می‌دهد</span>

</summary><pre style="font-size: 12px">
$collection->keyBy(
    $keyOrCallback // Key name or callback function to set as new keys
);
$collection->keyBy('id'); // Sets the 'id' field as key
$collection->keyBy(fn($item) => $item->slug); // Sets key using callback
</pre></details>
## Flattening / Collapsing: صاف کردن یا ترکیب مجموعه‌های چندبعدی
<details dir="ltr"><summary>
-> flatten($depth = INF); مجموعه چندبعدی را صاف می‌کند
</summary><pre style="font-size: 12px">
$collection->flatten(
    $depth = INF // The depth to flatten, default is INF for full flatten
);
collect([1, [2, [3, 4]]])->flatten(); // Returns [1, 2, 3, 4]
collect([1, [2, [3, 4]]])->flatten(1); // Returns [1, 2, [3, 4]]
</pre></details>
<details dir="ltr"><summary>
-> collapse(); مجموعه‌ای از آرایه‌ها را ترکیب می‌کند
</summary><pre style="font-size: 12px">
$collection->collapse();
collect([[1, 2], [3, 4]])->collapse(); // Returns [1, 2, 3, 4]
</pre></details>
<details dir="ltr"><summary>
-> collapseWithKeys(); مجموعه‌ای از آرایه‌ها را با حفظ کلیدها ترکیب می‌کند
</summary><pre style="font-size: 12px">
$collection->collapseWithKeys();
collect([['a' => 1], ['b' => 2]])->collapseWithKeys(); // Returns ['a' => 1, 'b' => 2]
</pre></details>
<details dir="ltr"><summary>
-> undot(); <span dir=rtl>ساختار dot-notated را به nested array بازمی‌گرداند</span>

</summary><pre style="font-size: 12px">
$collection->undot();
collect(['user.name' => 'John'])->undot(); // Returns ['user' => ['name' => 'John']]
</pre></details>
<details dir="ltr"><summary>
-> dot(); <span dir=rtl>مجموعه را به آرایه dot-notated تبدیل می‌کند</span>

</summary><pre style="font-size: 12px">
$collection->dot();
collect(['user' => ['name' => 'John']])->dot(); // Returns ['user.name' => 'John']
</pre></details>
<details dir="ltr"><summary>
-> flip(); کلید و مقدار مجموعه را جابجا می‌کند
</summary><pre style="font-size: 12px">
$collection->flip();
collect(['a' => 1, 'b' => 2])->flip(); // Returns [1 => 'a', 2 => 'b']
</pre></details>
## Combining / Merging: ترکیب چند مجموعه
<details dir="ltr"><summary>
-> combine($values); <span dir=rtl>دو مجموعه را به [key => value] ترکیب می‌کند</span>

</summary><pre style="font-size: 12px">
$collection->combine(
    $values // The values to combine with the keys of the collection
);
collect(['a', 'b'])->combine([1, 2]); // Returns ['a' => 1, 'b' => 2]
</pre></details>
<details dir="ltr"><summary>
-> concat($source); مجموعه دیگری را به انتهای مجموعه اضافه می‌کند
</summary><pre style="font-size: 12px">
$collection->concat(
    $source // Another collection or array to append
);
collect([1, 2])->concat([3, 4]); // Returns [1, 2, 3, 4]
</pre></details>
<details dir="ltr"><summary>
-> merge($source); ترکیب مجموعه‌ها
</summary><pre style="font-size: 12px">
$collection->merge(
    $source // Collection or array to merge
);
collect([1, 2])->merge([3, 4]); // Returns [1, 2, 3, 4]
</pre></details>
<details dir="ltr"><summary>
-> mergeRecursive($source); ترکیب بازگشتی مجموعه‌ها
</summary><pre style="font-size: 12px">
$collection->mergeRecursive(
    $source // Collection or array to merge recursively
);
collect(['a' => ['x' => 1]])->mergeRecursive(['a' => ['y' => 2]]); 
// Returns ['a' => ['x' => 1, 'y' => 2]]
</pre></details>
<details dir="ltr"><summary>
-> intersect($values); بر اساس مقادیر اشتراک می‌گیرد
</summary><pre style="font-size: 12px">
$collection->intersect(
    $values // Collection or array to compare values
);
collect([1, 2, 3])->intersect([2, 3, 4]); // Returns [2, 3]
</pre></details>
<details dir="ltr"><summary>
-> intersectUsing($values, $callback); <span dir=rtl>اشتراک با callback</span>

</summary><pre style="font-size: 12px">
$collection->intersectUsing(
    $values,   // Collection or array to compare values
    $callback  // Callback to determine equality
);
collect([1, 2, 3])->intersectUsing([2, 3, 4], fn($a, $b) => $a % 2 === $b % 2); 
// Returns [2]
</pre></details>
<details dir="ltr"><summary>
-> intersectAssoc($values); اشتراک بر اساس کلید و مقدار
</summary><pre style="font-size: 12px">
$collection->intersectAssoc(
    $values // Collection or array to compare key-value pairs
);
collect(['a' => 1, 'b' => 2])->intersectAssoc(['b' => 2, 'a' => 3]); 
// Returns ['b' => 2]
</pre></details>
<details dir="ltr"><summary>
-> intersectAssocUsing($values, $callback); <span dir=rtl>اشتراک کلید و مقدار با callback</span>

</summary><pre style="font-size: 12px">
$collection->intersectAssocUsing(
    $values,   // Collection or array to compare key-value pairs
    $callback  // Callback to determine equality
);
collect(['a' => 1, 'b' => 2])->intersectAssocUsing(['b' => 2, 'a' => 3], fn($a, $b) => $a % 2 === $b % 2); 
// Returns ['b' => 2]
</pre></details>
<details dir="ltr"><summary>
-> intersectByKeys($keys); اشتراک بر اساس کلیدها
</summary><pre style="font-size: 12px">
$collection->intersectByKeys(
    $keys // Collection, array, or keys to intersect with
);
collect(['a' => 1, 'b' => 2, 'c' => 3])->intersectByKeys(['b', 'c']); 
// Returns ['b' => 2, 'c' => 3]
</pre></details>
<details dir="ltr"><summary>
-> union($source); اتحاد مجموعه با مجموعه دیگر
</summary><pre style="font-size: 12px">
$collection->union(
    $source // Collection or array to unite with
);
collect(['a' => 1])->union(['b' => 2, 'a' => 3]); 
// Returns ['a' => 1, 'b' => 2]
</pre></details>
## Searching / Contains: جستجو و بررسی وجود
<details dir="ltr"><summary>
-> contains($valueOrCallback); بررسی وجود مقدار یا آیتم
</summary><pre style="font-size: 12px">
$collection->contains(
    $valueOrCallback // Value, key-value pair, or callback to check existence
);
collect([1, 2, 3])->contains(2); // Returns true
collect([1, 2, 3])->contains(fn($value) => $value > 2); // Returns true
</pre></details>
<details dir="ltr"><summary>
-> containsOneItem($value); بررسی وجود دقیق یک آیتم
</summary><pre style="font-size: 12px">
$collection->containsOneItem(
    $value // Exact value to check
);
collect([1, 2, 3, 2])->containsOneItem(2); // Returns false
collect([1, 2, 3])->containsOneItem(2); // Returns true
</pre></details>
<details dir="ltr"><summary>
-> containsStrict($value); بررسی وجود با مقایسه سخت
</summary><pre style="font-size: 12px">
$collection->containsStrict(
    $value // Value to check using strict comparison (===)
);
collect([1, 2, '3'])->containsStrict(3); // Returns false
collect([1, 2, 3])->containsStrict(3); // Returns true
</pre></details>
<details dir="ltr"><summary>
-> doesntContain($valueOrCallback); بررسی عدم وجود مقدار یا آیتم
</summary><pre style="font-size: 12px">
$collection->doesntContain(
    $valueOrCallback // Value, key-value pair, or callback to check non-existence
);
collect([1, 2, 3])->doesntContain(4); // Returns true
collect([1, 2, 3])->doesntContain(fn($value) => $value > 3); // Returns true
</pre></details>
<details dir="ltr"><summary>
-> doesntContainStrict($value); بررسی عدم وجود با مقایسه سخت
</summary><pre style="font-size: 12px">
$collection->doesntContainStrict(
    $value // Value to check using strict comparison (===)
);
collect([1, 2, '3'])->doesntContainStrict(3); // Returns true
collect([1, 2, 3])->doesntContainStrict(3); // Returns false
</pre></details>
<details dir="ltr"><summary>
-> search($value, $strict = false); جستجوی یک مقدار و برگرداندن کلید
</summary><pre style="font-size: 12px">
$collection->search(
    $value,  // Value to search for
    $strict = false // Whether to use strict comparison
);
collect([1, 2, 3])->search(2); // Returns 1
collect([1, 2, '3'])->search(3, true); // Returns false
</pre></details>
<details dir="ltr"><summary>
-> has($key); بررسی وجود کلید
</summary><pre style="font-size: 12px">
$collection->has(
    $key // Key or array of keys to check existence
);
collect(['name' => 'Alice', 'age' => 30])->has('age'); // Returns true
</pre></details>
<details dir="ltr"><summary>
-> hasAny($keys); بررسی وجود هر یک از کلیدها
</summary><pre style="font-size: 12px">
$collection->hasAny(
    $keys // Array of keys, returns true if any exist
);
collect(['name' => 'Alice', 'age' => 30])->hasAny(['email', 'age']); // Returns true
</pre></details>
## Count / Math / Statistics: شمارش و محاسبات
<details dir="ltr"><summary>
-> count(); تعداد آیتم‌ها
</summary><pre style="font-size: 12px">
$collection->count();
collect([1, 2, 3, 4])->count(); // Returns 4
</pre></details>
<details dir="ltr"><summary>
-> countBy($callback = null); <span dir=rtl>شمارش گروهی بر اساس callback یا کلید</span>
</summary><pre style="font-size: 12px">
$collection->countBy(
    $callback = null // Callback or key to group by before counting
);
collect([1, 2, 2, 3])->countBy(); // Returns [1 => 1, 2 => 2, 3 => 1]
collect(['apple', 'banana', 'apple'])->countBy(fn($item) => $item); // Returns ['apple' => 2, 'banana' => 1]
</pre></details>
<details dir="ltr"><summary>
-> sum($callback = null); جمع مقادیر
</summary><pre style="font-size: 12px">
$collection->sum(
    $callback = null // Optional callback to determine value for sum
);
collect([1, 2, 3, 4])->sum(); // Returns 10
collect([['price' => 100], ['price' => 200]])->sum(fn($item) => $item['price']); // Returns 300
</pre></details>
<details dir="ltr"><summary>
-> average($callback = null); میانگین
</summary><pre style="font-size: 12px">
$collection->average(
    $callback = null // Optional callback to determine value for average
);
collect([10, 20, 30])->average(); // Returns 20
</pre></details>
<details dir="ltr"><summary>
-> avg($callback = null); <span dir=rtl>میانگین (alias)</span>
</summary><pre style="font-size: 12px">
$collection->avg(
    $callback = null // Optional callback to determine value for average
);
collect([10, 20, 30])->avg(); // Returns 20
</pre></details>
<details dir="ltr"><summary>
-> max($callback = null); حداکثر مقدار
</summary><pre style="font-size: 12px">
$collection->max(
    $callback = null // Optional callback to determine value for comparison
);
collect([1, 3, 2, 5])->max(); // Returns 5
collect([['score' => 10], ['score' => 30]])->max(fn($item) => $item['score']); // Returns 30
</pre></details>
<details dir="ltr"><summary>
-> min($callback = null); حداقل مقدار
</summary><pre style="font-size: 12px">
$collection->min(
    $callback = null // Optional callback to determine value for comparison
);
collect([1, 3, 2, 5])->min(); // Returns 1
collect([['score' => 10], ['score' => 30]])->min(fn($item) => $item['score']); // Returns 10
</pre></details>
<details dir="ltr"><summary>
-> median($key = null); مقدار میانی
</summary><pre style="font-size: 12px">
$collection->median(
    $key = null // Optional key or callback to determine value
);
collect([1, 2, 3, 4, 5])->median(); // Returns 3
collect([['value' => 10], ['value' => 30], ['value' => 20]])->median('value'); // Returns 20
</pre></details>
<details dir="ltr"><summary>
-> mode($key = null); بیشترین مقدار
</summary><pre style="font-size: 12px">
$collection->mode(
    $key = null // Optional key or callback to determine value
);
collect([1, 2, 2, 3, 3, 3])->mode(); // Returns [3]
</pre></details>
<details dir="ltr"><summary>
-> multiply($multiplier); ضرب همه مقادیر
</summary><pre style="font-size: 12px">
$collection->multiply(
    $multiplier // Number to multiply each item by
);
collect([1, 2, 3])->multiply(2); // Returns [2, 4, 6]
</pre></details>
<details dir="ltr"><summary>
-> percentage($callback); درصد آیتم‌ها که شرط را برآورده می‌کنند
</summary><pre style="font-size: 12px">
$collection->percentage(
    $callback // Callback to check condition for each item
);
collect([1, 2, 3, 4, 5])->percentage(fn($item) => $item > 3); // Returns 40
</pre></details>
## Array / JSON Conversion: تبدیل به آرایه یا JSON
<details dir="ltr"><summary>
-> toArray(); <span dir=rtl>تبدیل به آرایه PHP</span>
</summary><pre style="font-size: 12px">
$collection->toArray();
collect([1, 2, 3])->toArray(); // Returns [1, 2, 3]
</pre></details>
<details dir="ltr"><summary>
-> toJson($options = 0); <span dir=rtl>تبدیل به JSON</span>
</summary><pre style="font-size: 12px">
$collection->toJson(
    $options = 0 // Optional JSON encode options
);
collect([1, 2, 3])->toJson(); // Returns '[1,2,3]'
</pre></details>
<details dir="ltr"><summary>
-> toPrettyJson(); <span dir=rtl>JSON زیبا</span>
</summary><pre style="font-size: 12px">
$collection->toPrettyJson();
collect([1, 2, 3])->toPrettyJson(); // Returns pretty printed JSON
</pre></details>
<details dir="ltr"><summary>
-> fromJson($json); <span dir=rtl>ایجاد مجموعه از JSON</span>

</summary><pre style="font-size: 12px">
$collection = collect()->fromJson(
    $json // JSON string to convert into Collection
);
collect()->fromJson('[1,2,3]'); // Returns Collection([1, 2, 3])
</pre></details>
<details dir="ltr"><summary>
-> collect($items); <span dir=rtl>ایجاد مجموعه از آرایه یا مقدار</span>
</summary><pre style="font-size: 12px">
$collection = collect(
    $items // Array or value to create collection from
);
collect([1, 2, 3]); // Returns Collection([1, 2, 3])
collect('value');   // Returns Collection(['value'])
</pre></details>
## Adding / Removing Items: اضافه یا حذف آیتم‌ها
<details dir="ltr"><summary>
-> push($item); افزودن آیتم به انتهای مجموعه
</summary><pre style="font-size: 12px">
$collection->push(
    $item // Item to add at the end of the collection
);
collect([1, 2, 3])->push(4); // Returns Collection([1, 2, 3, 4])
</pre></details>
<details dir="ltr"><summary>
-> prepend($item, $key = null); افزودن آیتم به ابتدای مجموعه
</summary><pre style="font-size: 12px">
$collection->prepend(
    $item, // Item to add at the beginning
    $key = null // Optional key for the item
);
collect([1, 2, 3])->prepend(0); // Returns Collection([0, 1, 2, 3])
collect([1, 2, 3])->prepend('first', 'a'); // Returns Collection(['a' => 'first', 1, 2, 3])
</pre></details>
<details dir="ltr"><summary>
-> pop(); حذف آیتم از انتها
</summary><pre style="font-size: 12px">
$removed = $collection->pop(); // Removes and returns the last item
collect([1, 2, 3])->pop(); // Returns 3, collection becomes [1, 2]
</pre></details>
<details dir="ltr"><summary>
-> shift(); حذف آیتم از ابتدا
</summary><pre style="font-size: 12px">
$removed = $collection->shift(); // Removes and returns the first item
collect([1, 2, 3])->shift(); // Returns 1, collection becomes [2, 3]
</pre></details>
<details dir="ltr"><summary>
-> put($key, $value); قرار دادن مقدار در کلید مشخص
</summary><pre style="font-size: 12px">
$collection->put(
    $key,   // Key to assign the value to
    $value  // Value to set
);
collect(['a' => 1, 'b' => 2])->put('c', 3); // Returns Collection(['a' => 1, 'b' => 2, 'c' => 3])
</pre></details>
<details dir="ltr"><summary>
-> pull($key, $default = null); حذف و برگرداندن مقدار کلید
</summary><pre style="font-size: 12px">
$value = $collection->pull(
    $key,       // Key to remove and return
    $default = null // Default value if key does not exist
);
$collection = collect(['a' => 1, 'b' => 2]);
$removed = $collection->pull('a'); // Returns 1, collection becomes ['b' => 2]
</pre></details>
<details dir="ltr"><summary>
-> forget($keys); حذف یک یا چند کلید
</summary><pre style="font-size: 12px">
$collection->forget(
    $keys // Single key or array of keys to remove
);
$collection = collect(['a' => 1, 'b' => 2, 'c' => 3]);
$collection->forget('b'); // Collection becomes ['a' => 1, 'c' => 3]
$collection->forget(['a', 'c']); // Collection becomes []
</pre></details>
<details dir="ltr"><summary>
-> replace($items); جایگزینی مقادیر
</summary><pre style="font-size: 12px">
$collection->replace(
    $items // Array or collection of new values
);
collect(['a' => 1, 'b' => 2])->replace(['b' => 20, 'c' => 30]); 
// Returns Collection(['a' => 1, 'b' => 20, 'c' => 30])
</pre></details>
<details dir="ltr"><summary>
-> replaceRecursive($items); جایگزینی بازگشتی مقادیر
</summary><pre style="font-size: 12px">
$collection->replaceRecursive(
    $items // Array or collection with nested values to merge recursively
);
$collection = collect(['a' => ['x' => 1], 'b' => 2]);
$collection->replaceRecursive(['a' => ['y' => 10], 'b' => 20]);
// Returns Collection(['a' => ['x' => 1, 'y' => 10], 'b' => 20])
</pre></details>
<details dir="ltr"><summary>
-> pad($size, $value); مجموعه را با مقدار مشخص پر می‌کند
</summary><pre style="font-size: 12px">
$collection->pad(
    $size,   // Desired total size
    $value   // Value to append if collection is smaller than size
);
collect([1, 2])->pad(5, 0); // Returns Collection([1, 2, 0, 0, 0])
</pre></details>
## Randomization / Ordering: ترتیب و تصادفی‌سازی
<details dir="ltr"><summary>
-> shuffle($seed = null); مرتب‌سازی تصادفی
</summary><pre style="font-size: 12px">
$collection->shuffle(
    $seed = null // Optional seed for reproducible random order
);
collect([1, 2, 3, 4])->shuffle(); // Returns items in random order
collect([1, 2, 3, 4])->shuffle(123); // Random order based on seed 123
</pre></details>
<details dir="ltr"><summary>
-> sort($callback = null); مرتب‌سازی
</summary><pre style="font-size: 12px">
$collection->sort(
    $callback = null // Optional callback to determine sort order
);
collect([4, 1, 3, 2])->sort(); // Returns Collection([1, 2, 3, 4])
collect([['name' => 'John'], ['name' => 'Jane']])
    ->sort(fn($a, $b) => strcmp($a['name'], $b['name'])); // Sort by name
</pre></details>
<details dir="ltr"><summary>
-> sortBy($callback, $options = SORT_REGULAR, $descending = false); <span dir=rtl>مرتب‌سازی بر اساس callback یا key</span>
</summary><pre style="font-size: 12px">
$collection->sortBy(
    $callback,     // Key or callback to sort by
    $options = SORT_REGULAR,  // Sorting type
    $descending = false       // Sort descending if true
);
collect([['name' => 'John'], ['name' => 'Jane']])
    ->sortBy('name'); // Sort by name ascending
collect([['score' => 50], ['score' => 70]])
    ->sortBy(fn($item) => $item['score'], SORT_REGULAR, true); // Descending
</pre></details>
<details dir="ltr"><summary>
-> sortByDesc($callback, $options = SORT_REGULAR); <span dir=rtl>مرتب‌سازی نزولی بر اساس callback یا کلید</span>
</summary><pre style="font-size: 12px">
$collection->sortByDesc(
    $callback,           // Key or callback to sort by
    $options = SORT_REGULAR // Sorting type
);
collect([['name' => 'John'], ['name' => 'Jane']])
    ->sortByDesc('name'); // Sort by name descending
</pre></details>
<details dir="ltr"><summary>
-> sortDesc($options = SORT_REGULAR); مرتب‌سازی نزولی ساده
</summary><pre style="font-size: 12px">
$collection->sortDesc(
    $options = SORT_REGULAR // Sorting type
);
collect([1, 3, 2, 4])->sortDesc(); // Returns Collection([4, 3, 2, 1])
</pre></details>
<details dir="ltr"><summary>
-> sortKeys($options = SORT_REGULAR); مرتب‌سازی بر اساس کلیدها
</summary><pre style="font-size: 12px">
$collection->sortKeys(
    $options = SORT_REGULAR // Sorting type
);
collect(['b' => 2, 'a' => 1])->sortKeys(); // Returns ['a' => 1, 'b' => 2]
</pre></details>
<details dir="ltr"><summary>
-> sortKeysDesc($options = SORT_REGULAR); مرتب‌سازی بر اساس کلیدها نزولی
</summary><pre style="font-size: 12px">
$collection->sortKeysDesc(
    $options = SORT_REGULAR // Sorting type
);
collect(['a' => 1, 'b' => 2])->sortKeysDesc(); // Returns ['b' => 2, 'a' => 1]
</pre></details>
<details dir="ltr"><summary>
-> sortKeysUsing($callback); <span dir=rtl>مرتب‌سازی کلیدها با callback</span>
</summary><pre style="font-size: 12px">
$collection->sortKeysUsing(
    $callback // Callback to compare keys
);
collect(['b' => 2, 'a' => 1])
    ->sortKeysUsing(fn($a, $b) => strcmp($a, $b)); // Custom key sort
</pre></details>
<details dir="ltr"><summary>
-> reverse(); برعکس کردن ترتیب
</summary><pre style="font-size: 12px">
$collection->reverse();
collect([1, 2, 3])->reverse(); // Returns Collection([3, 2, 1])
</pre></details>
## Iteration / Lazy Execution: پیمایش و اجرای تنبل
<details dir="ltr"><summary>
-> each($callback); <span dir=rtl>اجرای callback روی هر آیتم</span>
</summary><pre style="font-size: 12px">
$collection->each(
    $callback // Function to apply to each item
);
collect([1, 2, 3])->each(fn($item) => print($item)); // Prints 1 2 3
</pre></details>
<details dir="ltr"><summary>
-> eachSpread($callback); <span dir=rtl>callback روی آرایه‌های چندبعدی</span>
</summary><pre style="font-size: 12px">
$collection->eachSpread(
    $callback // Function to apply to each sub-array
);
collect([[1, 2], [3, 4]])
    ->eachSpread(fn($a, $b) => print($a + $b)); // Prints 3 7
</pre></details>
<details dir="ltr"><summary>
-> every($callback); بررسی صحت شرط برای همه آیتم‌ها
</summary><pre style="font-size: 12px">
$collection->every(
    $callback // Function returning boolean for each item
);
collect([2, 4, 6])->every(fn($value) => $value % 2 === 0); // Returns true
</pre></details>
<details dir="ltr"><summary>
-> some($callback); بررسی صحت شرط برای حداقل یک آیتم
</summary><pre style="font-size: 12px">
$collection->some(
    $callback // Function returning boolean for each item
);
collect([1, 3, 4])->some(fn($value) => $value % 2 === 0); // Returns true
</pre></details>
<details dir="ltr"><summary>
-> tap($callback); <span dir=rtl>اجرای callback و بازگرداندن مجموعه</span>
</summary><pre style="font-size: 12px">
$collection->tap(
    $callback // Function to perform side effects
);
collect([1, 2, 3])->tap(fn($c) => print(count($c))); // Prints 3 and returns collection
</pre></details>
<details dir="ltr"><summary>
-> lazy(); <span dir=rtl>ایجاد LazyCollection</span>
</summary><pre style="font-size: 12px">
$collection->lazy();
collect([1, 2, 3])->lazy(); // Returns a LazyCollection
</pre></details>
<details dir="ltr"><summary>
-> times($number, $callback); ایجاد مجموعه با تعداد مشخص
</summary><pre style="font-size: 12px">
Collection::times(
    $number,   // Number of times to execute callback
    $callback  // Function returning item value
);
Collection::times(3, fn($i) => $i * 2); // Returns Collection([2, 4, 6])
</pre></details>
## Utilities / Miscellaneous: ابزارها و متفرقه
<details dir="ltr"><summary>
-> macro($name, $callback); <span dir=rtl>افزودن متد دلخواه به Collection</span>
</summary><pre style="font-size: 12px">
Collection::macro(
    $name,      // Name of the custom method
    $callback   // Function implementing the method
);
Collection::macro('double', fn($collection) => $collection->map(fn($i) => $i * 2));
// Now all collections have ->double() method
</pre></details>
<details dir="ltr"><summary>
-> make($items); ایجاد مجموعه از آیتم‌ها
</summary><pre style="font-size: 12px">
Collection::make(
    $items // Array or items to create a collection from
);
$collection = Collection::make([1, 2, 3]); // Creates a collection with 3 items
</pre></details>
<details dir="ltr"><summary>
-> value(); برگرداندن مقدار مجموعه وقتی فقط یک آیتم دارد
</summary><pre style="font-size: 12px">
$collection->value();
collect([42])->value(); // Returns 42
</pre></details>
<details dir="ltr"><summary>
-> values(); بازنشانی کلیدها
</summary><pre style="font-size: 12px">
$collection->values();
collect([3 => 'a', 7 => 'b'])->values(); // Returns ['a', 'b'] with keys reset
</pre></details>
<details dir="ltr"><summary>
-> wrap($value); <span dir=rtl>تبدیل مقدار به Collection اگر نباشد</span>
</summary><pre style="font-size: 12px">
Collection::wrap(
    $value // Single value or array
);
Collection::wrap(5); // Returns Collection([5])
Collection::wrap([1, 2]); // Returns Collection([1, 2])
</pre></details>
<details dir="ltr"><summary>
-> implode($keyOrGlue, $glue = null); <span dir=rtl>تبدیل به رشته با glue</span>
</summary><pre style="font-size: 12px">
$collection->implode(
    $keyOrGlue, // Key or glue string
    $glue = null // Optional glue if first parameter is key
);
collect(['name' => 'Alice', 'age' => 25])->implode(', '); // Returns "Alice, 25"
</pre></details>
## Debug / Dump: نمایش و اشکال‌زدایی
<details dir="ltr"><summary>
-> dd(); dump و die
</summary><pre style="font-size: 12px">
$collection->dd();
// Dumps the collection contents and stops execution
collect([1, 2, 3])->dd(); // Outputs the array and stops
</pre></details>
<details dir="ltr"><summary>
-> dump(); نمایش مقادیر
</summary><pre style="font-size: 12px">
$collection->dump();
// Dumps the collection contents without stopping execution
collect([1, 2, 3])->dump(); // Outputs the array but continues execution
</pre></details>
## Misc: سایر متدهای ترکیبی یا پیشرفته
<details dir="ltr"><summary>
-> pluck($valueOrKey, $key = null) :Array; استخراج مقادیر از مجموعه
</summary><pre style="font-size: 12px">
$collection->pluck(
    $valueOrKey,    // The value field or callback to extract
    $key = null     // Optional key to use for the resulting collection
);
$collection = collect([
    ['name' => 'Alice', 'age' => 25],
    ['name' => 'Bob', 'age' => 30]
]);
$names = $collection->pluck('name'); // ['Alice', 'Bob']
$ages = $collection->pluck('age', 'name'); // ['Alice' => 25, 'Bob' => 30]
</pre></details>
<details dir="ltr"><summary>
$collection1->zip($collection2) :Collection; ترکیب مجموعه‌ها به صورت جفتی
</summary><pre style="font-size: 12px">
$collection->zip($items);
// Combines two collections together, pairing values by index
$collection1 = collect([1, 2, 3]);
$collection2 = collect(['a', 'b', 'c']);
$zipped = $collection1->zip($collection2); // [[1, 'a'], [2, 'b'], [3, 'c']]
</pre></details>
<details dir="ltr"><summary>
-> range($start, $end, $step = 1) :simpleArray; ایجاد مجموعه بازه‌ای
</summary><pre style="font-size: 12px">
$collection->range(
    $start,    // Starting number
    $end,      // Ending number
    $step = 1  // Increment step
);
$numbers = collect()->range(1, 5); // [1, 2, 3, 4, 5]
$numbersWithStep = collect()->range(0, 10, 2); // [0, 2, 4, 6, 8, 10]
</pre></details>
<details dir="ltr"><summary>
-> pipe($callback); <span dir=rtl>عبور مجموعه از callback</span>
</summary><pre style="font-size: 12px">
$collection->pipe(
    $callback  // Callback function to process the collection
);
$collection = collect([1, 2, 3]);
$result = $collection->pipe(function ($col) {
    return $col->sum();
}); // 6
</pre></details>
<details dir="ltr"><summary>
-> pipeInto($className); عبور مجموعه به سازنده کلاس
</summary><pre style="font-size: 12px">
$collection->pipeInto(
    $className  // Class name to pass collection into constructor
);
$collection = collect([1, 2, 3]);
$object = $collection->pipeInto(SomeClass::class);
</pre></details>
<details dir="ltr"><summary>
-> pipeThrough($pipes); <span dir=rtl>عبور مجموعه از چند callback</span>
</summary><pre style="font-size: 12px">
$collection->pipeThrough(
    $pipes  // Array of callback functions to process the collection sequentially
);
$collection = collect([1, 2, 3]);
$result = $collection->pipeThrough([
    fn($col) => $col->map(fn($x) => $x * 2),
    fn($col) => $col->filter(fn($x) => $x > 2)
]); // [4, 6]
</pre></details>

