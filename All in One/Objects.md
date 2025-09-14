Object Methods in PHP and JavaScript
## Get Keys
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array_keys(array $array)  
<span style="color:gray;">: array // returns all keys of an array/object</span>  
</summary>
<bdi class="fa">تمام کلیدهای آرایه یا object را برمی‌گرداند</bdi>
<pre style="font-size: 12px">
$person = ["name" => "Ali", "age" => 30];
print_r(array_keys($person));
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Object.keys(obj: object)  
<span style="color:gray;">: string[] // returns array of keys</span>  
</summary>
<bdi class="fa">کلیدهای یک شیء را در آرایه‌ای برمی‌گرداند</bdi>
<pre style="font-size: 12px">
let person = {name: "Ali", age: 30};
console.log(Object.keys(person));
</pre></details>
## Get Values
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array_values(array $array)  
<span style="color:gray;">: array // returns all values of an array/object</span>  
</summary>
<bdi class="fa">تمام مقادیر را در آرایه‌ای برمی‌گرداند</bdi>
<pre style="font-size: 12px">
$person = ["name" => "Ali", "age" => 30];
print_r(array_values($person));
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Object.values(obj: object)  
<span style="color:gray;">: any[] // returns array of values</span>  
</summary>
<bdi class="fa">مقادیر یک شیء را در آرایه‌ای برمی‌گرداند</bdi>
<pre style="font-size: 12px">
let person = {name: "Ali", age: 30};
console.log(Object.values(person));
</pre></details>
## Get Key-Value Pairs
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
(array) iterable (foreach)  
<span style="color:gray;">: yields key => value</span>  
</summary>
<bdi class="fa">در PHP با foreach کلید-مقدار را به دست می‌آوریم</bdi>
<pre style="font-size: 12px">
$person = ["name" => "Ali", "age" => 30];
foreach ($person as $key => $value) {
    echo "$key : $value\n";
}
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Object.entries(obj: object)  
<span style="color:gray;">: [string, any][] // returns array of key-value pairs</span>  
</summary>
<bdi class="fa">کلید و مقدارها را به‌صورت آرایه دوبخشی برمی‌گرداند</bdi>
<pre style="font-size: 12px">
let person = {name: "Ali", age: 30};
console.log(Object.entries(person));
</pre></details>
## Check Property Exists
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array_key_exists(string|int $key, array $array)  
<span style="color:gray;">: bool // checks if key exists</span>  
</summary>
<bdi class="fa">بررسی وجود کلید در آرایه یا object</bdi>
<pre style="font-size: 12px">
$person = ["name" => "Ali"];
var_dump(array_key_exists("name", $person)); // true
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
("key" in obj)  
<span style="color:gray;">: boolean // true if property exists</span>  
</summary>
<bdi class="fa">بررسی وجود کلید در شیء</bdi>
<pre style="font-size: 12px">
let person = {name: "Ali"};
console.log("name" in person); // true
</pre></details>
## Merge Objects
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
array_merge(array ...$arrays)  
<span style="color:gray;">: array // merges arrays</span>  
</summary>
<bdi class="fa">دو آرایه (object associative) را ادغام می‌کند</bdi>
<pre style="font-size: 12px">
$a = ["name" => "Ali"];
$b = ["age" => 30];
print_r(array_merge($a, $b));
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Object.assign(target: object, ...sources: object[])  
<span style="color:gray;">: object // merges objects</span>  
</summary>
<bdi class="fa">چند شیء را ادغام می‌کند</bdi>
<pre style="font-size: 12px">
let a = {name: "Ali"};
let b = {age: 30};
console.log(Object.assign({}, a, b));
</pre></details>
## Convert to JSON
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
json_encode(mixed $value)  
<span style="color:gray;">: string (JSON)</span>  
</summary>
<bdi class="fa">آبجکت یا آرایه را به JSON تبدیل می‌کند</bdi>
<pre style="font-size: 12px">
$person = ["name" => "Ali", "age" => 30];
echo json_encode($person);
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
JSON.stringify(obj: any)  
<span style="color:gray;">: string (JSON)</span>  
</summary>
<bdi class="fa">آبجکت را به JSON تبدیل می‌کند</bdi>
<pre style="font-size: 12px">
let person = {name: "Ali", age: 30};
console.log(JSON.stringify(person));
</pre></details>
## Parse JSON
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
json_decode(string $json, bool $assoc = false)  
<span style="color:gray;">: mixed (object or array)</span>  
</summary>
<bdi class="fa">JSON را به object یا array برمی‌گرداند</bdi>
<pre style="font-size: 12px">
$json = '{"name":"Ali","age":30}';
print_r(json_decode($json, true));
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
JSON.parse(text: string)  
<span style="color:gray;">: any (object/array)</span>  
</summary>
<bdi class="fa">رشته JSON را به object تبدیل می‌کند</bdi>
<pre style="font-size: 12px">
let json = '{"name":"Ali","age":30}';
console.log(JSON.parse(json));
</pre></details>