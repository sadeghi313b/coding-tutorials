📘 PHP & JavaScript Array Methods (Side by Side)
## Push / Append
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array_push(array &$array, mixed $value1, mixed ...$values)  
<span style="color:gray;">: int (new array length) // adds elements to the end</span>  
</summary>
<bdi class="fa">یک یا چند عضو به انتهای آرایه اضافه می‌کند</bdi>
<pre style="font-size: 12px">
$numbers = [1,2]; array_push($numbers, 3);
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array.push(element1: any, ...elements: any[])  
<span style="color:gray;">: number (new array length) // adds elements to the end</span>  
</summary>
<bdi class="fa">یک یا چند عضو به انتهای آرایه اضافه می‌کند</bdi>
<pre style="font-size: 12px">
let numbers = [1,2]; numbers.push(3);
</pre></details>
## Pop / Remove last
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array_pop(array &$array)  
<span style="color:gray;">: mixed (last element) // removes and returns last element</span>  
</summary>
<bdi class="fa">آخرین عضو آرایه را حذف کرده و همان را برمی‌گرداند</bdi>
<pre style="font-size: 12px">
$last = array_pop($numbers);
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array.pop()  
<span style="color:gray;">: any (last element) // removes and returns last element</span>  
</summary>
<bdi class="fa">آخرین عضو آرایه را حذف کرده و همان را برمی‌گرداند</bdi>
<pre style="font-size: 12px">
let last = numbers.pop();
</pre></details>
## Shift / Remove first
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array_shift(array &$array)  
<span style="color:gray;">: mixed (first element) // removes and returns first element</span>  
</summary>
<bdi class="fa">اولین عضو آرایه را حذف کرده و همان را برمی‌گرداند</bdi>
<pre style="font-size: 12px">
$first = array_shift($numbers);
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array.shift()  
<span style="color:gray;">: any (first element) // removes and returns first element</span>  
</summary>
<bdi class="fa">اولین عضو آرایه را حذف کرده و همان را برمی‌گرداند</bdi>
<pre style="font-size: 12px">
let first = numbers.shift();
</pre></details>
## Unshift / Prepend
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array_unshift(array &$array, mixed $value1, mixed ...$values)  
<span style="color:gray;">: int (new array length) // adds elements to the beginning</span>  
</summary>
<bdi class="fa">یک یا چند عضو به ابتدای آرایه اضافه می‌کند</bdi>
<pre style="font-size: 12px">
array_unshift($numbers, 0);
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array.unshift(element1: any, ...elements: any[])  
<span style="color:gray;">: number (new array length) // adds elements to the beginning</span>  
</summary>
<bdi class="fa">یک یا چند عضو به ابتدای آرایه اضافه می‌کند</bdi>
<pre style="font-size: 12px">
numbers.unshift(0);
</pre></details>
## Includes / In Array
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
in_array(mixed $needle, array $haystack, bool $strict = false)  
<span style="color:gray;">: bool // checks if value exists in array</span>  
</summary>
<bdi class="fa">بررسی می‌کند که آیا مقدار در آرایه وجود دارد یا خیر</bdi>
<pre style="font-size: 12px">
in_array(3, $numbers);
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array.includes(searchElement: any, fromIndex?: number)  
<span style="color:gray;">: boolean // checks if value exists in array</span>  
</summary>
<bdi class="fa">بررسی می‌کند که آیا مقدار در آرایه وجود دارد یا خیر</bdi>
<pre style="font-size: 12px">
numbers.includes(3);
</pre></details>
## Search / Index
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array_search(mixed $needle, array $haystack, bool $strict = false)  
<span style="color:gray;">: int|string|false // index of element or false</span>  
</summary>
<bdi class="fa">ایندکس یک مقدار را پیدا می‌کند</bdi>
<pre style="font-size: 12px">
array_search(2, $numbers);
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array.indexOf(searchElement: any, fromIndex?: number)  
<span style="color:gray;">: number // index of element or -1</span>  
</summary>
<bdi class="fa">ایندکس یک مقدار را پیدا می‌کند</bdi>
<pre style="font-size: 12px">
numbers.indexOf(2);
</pre></details>
## Sorting
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
sort(array &$array, int $sort_flags = SORT_REGULAR)  
<span style="color:gray;">: bool // sorts ascending</span>  
rsort(array &$array, int $sort_flags = SORT_REGULAR)  
<span style="color:gray;">: bool // sorts descending</span>  
</summary>
<bdi class="fa">مرتب‌سازی آرایه (صعودی / نزولی)</bdi>
<pre style="font-size: 12px">
sort($numbers);
rsort($numbers);
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array.sort(compareFn?: (a: any, b: any) => number)  
<span style="color:gray;">: any[] // sorts ascending by default</span>  
array.reverse()  
<span style="color:gray;">: any[] // reverses array order</span>  
</summary>
<bdi class="fa">مرتب‌سازی آرایه (صعودی / معکوس)</bdi>
<pre style="font-size: 12px">
numbers.sort((a,b)=>a-b);
numbers.reverse();
</pre></details>
## Mapping
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array_map(callable $callback, array $array, array ...$arrays)  
<span style="color:gray;">: array // transformed array</span>  
</summary>
<bdi class="fa">روی هر عضو تابعی اعمال کرده و آرایه جدید می‌سازد</bdi>
<pre style="font-size: 12px">
$squared = array_map(fn($n)=>$n*$n, $numbers);
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array.map(callback: (element: any, index: number, array: any[]) => any)  
<span style="color:gray;">: any[] // transformed array</span>  
</summary>
<bdi class="fa">روی هر عضو تابعی اعمال کرده و آرایه جدید می‌سازد</bdi>
<pre style="font-size: 12px">
let squared = numbers.map(n => n*n);
</pre></details>
## Filtering
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array_filter(array $array, ?callable $callback = null, int $mode = 0)  
<span style="color:gray;">: array // filtered array</span>  
</summary>
<bdi class="fa">اعضا را بر اساس شرط فیلتر می‌کند</bdi>
<pre style="font-size: 12px">
$even = array_filter($numbers, fn($n)=>$n%2===0);
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array.filter(callback: (element: any, index: number, array: any[]) => boolean)  
<span style="color:gray;">: any[] // filtered array</span>  
</summary>
<bdi class="fa">اعضا را بر اساس شرط فیلتر می‌کند</bdi>
<pre style="font-size: 12px">
let even = numbers.filter(n => n%2===0);
</pre></details>
## Merge / Concat
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array_merge(array ...$arrays)  
<span style="color:gray;">: array // merged array</span>  
</summary>
<bdi class="fa">چند آرایه را با هم ترکیب می‌کند</bdi>
<pre style="font-size: 12px">
$all = array_merge([1,2],[3,4]);
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array.concat(...values: ConcatArray<any>[])  
<span style="color:gray;">: any[] // merged array</span>  
</summary>
<bdi class="fa">چند آرایه را با هم ترکیب می‌کند</bdi>
<pre style="font-size: 12px">
let all = [1,2].concat([3,4]);
</pre></details>
## Slice / Splice
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array_slice(array $array, int $offset, ?int $length = null, bool $preserve_keys = false)  
<span style="color:gray;">: array // subarray</span>  
</summary>
<bdi class="fa">بخشی از آرایه را جدا می‌کند</bdi>
<pre style="font-size: 12px">
$part = array_slice([1,2,3,4], 1, 2);
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array.slice(start?: number, end?: number)  
<span style="color:gray;">: any[] // subarray</span>  
array.splice(start: number, deleteCount?: number, ...items: any[])  
<span style="color:gray;">: any[] // removed elements</span>  
</summary>
<bdi class="fa">بخشی از آرایه را جدا یا جایگزین می‌کند</bdi>
<pre style="font-size: 12px">
let part = [1,2,3,4].slice(1,3);
let removed = [1,2,3,4].splice(1,2,99);
</pre></details>
## Join / Implode
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
implode(string $separator, array $array)  
<span style="color:gray;">: string // array to string</span>  
</summary>
<bdi class="fa">آرایه را به رشته تبدیل می‌کند</bdi>
<pre style="font-size: 12px">
$str = implode(',', [1,2,3]);
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array.join(separator?: string)  
<span style="color:gray;">: string // array to string</span>  
</summary>
<bdi class="fa">آرایه را به رشته تبدیل می‌کند</bdi>
<pre style="font-size: 12px">
let str = [1,2,3].join(',');
</pre></details>
## Other Useful
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array_unique(array $array)  
<span style="color:gray;">: array // removes duplicates</span>  
</summary>
<bdi class="fa">مقادیر تکراری را حذف می‌کند</bdi>
<pre style="font-size: 12px">
$uniq = array_unique([1,2,2,3]);
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array.find(callback: (element:any)=>boolean)  
<span style="color:gray;">: any // first matching element</span>  
</summary>
<bdi class="fa">اولین عضو مطابق شرط را پیدا می‌کند</bdi>
<pre style="font-size: 12px">
let firstEven = [1,2,3].find(n=>n%2===0);
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array.reduce(callback: (acc:any,cur:any)=>any, initialValue:any)  
<span style="color:gray;">: any // accumulated result</span>  
</summary>
<bdi class="fa">آرایه را به یک مقدار واحد کاهش می‌دهد</bdi>
<pre style="font-size: 12px">
let sum = [1,2,3].reduce((a,b)=>a+b,0);
</pre></details>