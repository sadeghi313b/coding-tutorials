# JSON Methods in PHP and JavaScript
## Encode / Convert to JSON
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
json_encode(mixed $value, int $options = 0, int $depth = 512)  
<span style="color:gray;">: string // converts value to JSON string</span>  
</summary>
<bdi class="fa">آرایه یا شیء را به JSON تبدیل می‌کند</bdi>
<pre style="font-size: 12px">
$person = ["name"=>"Ali","age"=>30];
echo json_encode($person); // {"name":"Ali","age":30}
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
JSON.stringify(value: any, replacer?: any, space?: string|number)  
<span style="color:gray;">: string // converts object/array to JSON string</span>  
</summary>
<bdi class="fa">شیء یا آرایه را به رشته JSON تبدیل می‌کند</bdi>
<pre style="font-size: 12px">
let person = {name:"Ali", age:30};
console.log(JSON.stringify(person)); // {"name":"Ali","age":30}
</pre></details>
## Decode / Parse JSON
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
json_decode(string $json, bool $assoc = false, int $depth = 512, int $flags = 0)  
<span style="color:gray;">: mixed // converts JSON string to array or object</span>  
</summary>
<bdi class="fa">رشته JSON را به آرایه یا شیء تبدیل می‌کند</bdi>
<pre style="font-size: 12px">
$json = '{"name":"Ali","age":30}';
$data = json_decode($json, true); // as associative array
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
JSON.parse(text: string, reviver?: (key: string, value: any) => any)  
<span style="color:gray;">: any // converts JSON string to object/array</span>  
</summary>
<bdi class="fa">رشته JSON را به شیء یا آرایه تبدیل می‌کند</bdi>
<pre style="font-size: 12px">
let json = '{"name":"Ali","age":30}';
let data = JSON.parse(json);
</pre></details>
## Pretty Print / Formatting
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
json_encode(mixed $value, int $options = JSON_PRETTY_PRINT)  
<span style="color:gray;">: string // JSON with indentation</span>  
</summary>
<bdi class="fa">تبدیل آرایه یا شیء به JSON با فرمت خوانا</bdi>
<pre style="font-size: 12px">
$person = ["name"=>"Ali","age"=>30];
echo json_encode($person, JSON_PRETTY_PRINT);
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
JSON.stringify(value: any, replacer?: any, space: number|string = 2)  
<span style="color:gray;">: string // JSON with indentation</span>  
</summary>
<bdi class="fa">تبدیل شیء یا آرایه به JSON با فرمت خوانا</bdi>
<pre style="font-size: 12px">
let person = {name:"Ali", age:30};
console.log(JSON.stringify(person, null, 2));
</pre></details>
## Handling Arrays/Objects in JSON
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
is_array(mixed $var)  
<span style="color:gray;">: bool // checks if variable is array</span>  
</summary>
<bdi class="fa">بررسی می‌کند که متغیر آرایه است یا خیر</bdi>
<pre style="font-size: 12px">
$data = json_decode($json, true);
if (is_array($data)) { /* do something */ }
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Array.isArray(obj: any)  
<span style="color:gray;">: boolean // checks if object is array</span>  
</summary>
<bdi class="fa">بررسی می‌کند که آیا متغیر آرایه است یا خیر</bdi>
<pre style="font-size: 12px">
let data = JSON.parse(json);
console.log(Array.isArray(data)); // true/false
</pre></details>
## Accessing Properties
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
$array["key"]  
<span style="color:gray;">: mixed // access value by key</span>  
</summary>
<bdi class="fa">دسترسی به مقدار با کلید در آرایه/شیء PHP</bdi>
<pre style="font-size: 12px">
$data = json_decode($json, true);
echo $data["name"]; // Ali
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
obj.key or obj["key"]  
<span style="color:gray;">: any // access value by key</span>  
</summary>
<bdi class="fa">دسترسی به مقدار با کلید در شیء JS</bdi>
<pre style="font-size: 12px">
let data = JSON.parse(json);
console.log(data.name); // Ali
</pre></details>
## Modify / Add Properties
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
$array["newKey"] = "value";  
<span style="color:gray;">: void // adds or modifies key</span>  
</summary>
<bdi class="fa">اضافه کردن یا تغییر مقدار کلید در آرایه/شیء PHP</bdi>
<pre style="font-size: 12px">
$data["city"] = "Tehran";
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
obj.newKey = value; or obj["newKey"] = value;  
<span style="color:gray;">: void // adds or modifies key</span>  
</summary>
<bdi class="fa">اضافه کردن یا تغییر مقدار کلید در شیء JS</bdi>
<pre style="font-size: 12px">
data.city = "Tehran";
</pre></details>


