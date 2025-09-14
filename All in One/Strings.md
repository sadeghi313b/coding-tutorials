# String Methods in PHP and JavaScript
## Length of String
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
strlen(string $string)  
<span style="color:gray;">: int // returns length of string</span>  
</summary>
<bdi class="fa">طول رشته را برمی‌گرداند</bdi>
<pre style="font-size: 12px">
echo strlen("Hello"); // 5
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
string.length  
<span style="color:gray;">: number // returns length of string</span>  
</summary>
<bdi class="fa">طول رشته را برمی‌گرداند</bdi>
<pre style="font-size: 12px">
console.log("Hello".length); // 5
</pre></details>
## Convert Case
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
strtoupper(string $string)  
<span style="color:gray;">: string // converts to uppercase</span>  
strtolower(string $string)  
<span style="color:gray;">: string // converts to lowercase</span>  
</summary>
<bdi class="fa">تبدیل حروف به بزرگ یا کوچک</bdi>
<pre style="font-size: 12px">
echo strtoupper("hello"); // HELLO
echo strtolower("WORLD"); // world
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
string.toUpperCase()  
<span style="color:gray;">: string</span>  
string.toLowerCase()  
<span style="color:gray;">: string</span>  
</summary>
<bdi class="fa">تبدیل حروف به بزرگ یا کوچک</bdi>
<pre style="font-size: 12px">
console.log("hello".toUpperCase()); // HELLO
console.log("WORLD".toLowerCase()); // world
</pre></details>
## Trim Spaces
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
trim(string $string)  
<span style="color:gray;">: string // removes whitespace both sides</span>  
ltrim(string $string)  
<span style="color:gray;">: string // removes left whitespace</span>  
rtrim(string $string)  
<span style="color:gray;">: string // removes right whitespace</span>  
</summary>
<bdi class="fa">حذف فاصله‌های ابتدا و انتها</bdi>
<pre style="font-size: 12px">
echo trim("  hi  "); // "hi"
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
string.trim()  
<span style="color:gray;">: string</span>  
string.trimStart()  
<span style="color:gray;">: string</span>  
string.trimEnd()  
<span style="color:gray;">: string</span>  
</summary>
<bdi class="fa">حذف فاصله‌های ابتدا و انتها</bdi>
<pre style="font-size: 12px">
console.log("  hi  ".trim()); // "hi"
</pre></details>
## Substring
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
substr(string $string, int $start, ?int $length=null)  
<span style="color:gray;">: string // substring</span>  
</summary>
<bdi class="fa">بخشی از رشته را برمی‌گرداند</bdi>
<pre style="font-size: 12px">
echo substr("Hello", 1, 3); // ell
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
string.substring(start: number, end?: number)  
<span style="color:gray;">: string</span>  
string.slice(start: number, end?: number)  
<span style="color:gray;">: string</span>  
</summary>
<bdi class="fa">بخشی از رشته را برمی‌گرداند</bdi>
<pre style="font-size: 12px">
console.log("Hello".substring(1, 4)); // ell
console.log("Hello".slice(1, 4));     // ell
</pre></details>
## Search Position
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
strpos(string $haystack, string $needle)  
<span style="color:gray;">: int|false // first occurrence index</span>  
strrpos(string $haystack, string $needle)  
<span style="color:gray;">: int|false // last occurrence index</span>  
</summary>
<bdi class="fa">پیدا کردن موقعیت اولین/آخرین occurrence</bdi>
<pre style="font-size: 12px">
echo strpos("Hello World", "o");  // 4
echo strrpos("Hello World", "o"); // 7
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
string.indexOf(searchValue: string, fromIndex?: number)  
<span style="color:gray;">: number</span>  
string.lastIndexOf(searchValue: string)  
<span style="color:gray;">: number</span>  
</summary>
<bdi class="fa">پیدا کردن موقعیت اولین/آخرین occurrence</bdi>
<pre style="font-size: 12px">
console.log("Hello World".indexOf("o"));     // 4
console.log("Hello World".lastIndexOf("o")); // 7
</pre></details>
## Replace
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
str_replace(string|array $search, string|array $replace, string $subject)  
<span style="color:gray;">: string // replaces occurrences</span>  
</summary>
<bdi class="fa">جایگزینی زیررشته‌ها</bdi>
<pre style="font-size: 12px">
echo str_replace("World", "PHP", "Hello World"); // Hello PHP
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
string.replace(searchValue: string|RegExp, replaceValue: string)  
<span style="color:gray;">: string</span>  
string.replaceAll(searchValue: string|RegExp, replaceValue: string)  
<span style="color:gray;">: string</span>  
</summary>
<bdi class="fa">جایگزینی زیررشته‌ها</bdi>
<pre style="font-size: 12px">
console.log("Hello World".replace("World", "JS")); // Hello JS
</pre></details>
## Split & Join
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
explode(string $delimiter, string $string)  
<span style="color:gray;">: array // splits string into array</span>  
implode(string $glue, array $pieces)  
<span style="color:gray;">: string // joins array into string</span>  
</summary>
<bdi class="fa">تقسیم رشته یا پیوند آرایه به رشته</bdi>
<pre style="font-size: 12px">
print_r(explode(",", "a,b,c")); // ["a","b","c"]
echo implode("-", ["a","b","c"]); // a-b-c
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
string.split(separator: string|RegExp, limit?: number)  
<span style="color:gray;">: string[]</span>  
array.join(separator?: string)  
<span style="color:gray;">: string</span>  
</summary>
<bdi class="fa">تقسیم رشته یا پیوند آرایه به رشته</bdi>
<pre style="font-size: 12px">
console.log("a,b,c".split(",")); // ["a","b","c"]
console.log(["a","b","c"].join("-")); // a-b-c
</pre></details>