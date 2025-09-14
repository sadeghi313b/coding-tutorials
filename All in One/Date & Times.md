# Date & Time Methods in PHP and JavaScript
## Current Date & Time
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
date(string $format, int|null $timestamp = null)  
<span style="color:gray;">: string // formats current or given timestamp</span>  
</summary>
<bdi class="fa">تاریخ و زمان جاری یا timestamp داده شده را برمی‌گرداند</bdi>
<pre style="font-size: 12px">
echo date("Y-m-d H:i:s"); // 2025-09-14 06:15:00
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
new Date()  
<span style="color:gray;">: Date // current date and time</span>  
</summary>
<bdi class="fa">تاریخ و زمان جاری را ایجاد می‌کند</bdi>
<pre style="font-size: 12px">
let now = new Date();
console.log(now); // 2025-09-14T03:15:00.000Z
</pre></details>
## Timestamp / Unix Time
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
time()  
<span style="color:gray;">: int // current Unix timestamp</span>  
</summary>
<bdi class="fa">تاریخ جاری به صورت timestamp Unix</bdi>
<pre style="font-size: 12px">
echo time(); // 1694645700
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
Date.now()  
<span style="color:gray;">: number // milliseconds since 1970-01-01</span>  
</summary>
<bdi class="fa">تاریخ جاری به صورت timestamp (میلی‌ثانیه) از 1970</bdi>
<pre style="font-size: 12px">
console.log(Date.now()); // 1694645700000
</pre></details>
## Create Specific Date
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
mktime(int $hour, int $minute, int $second, int $month, int $day, int $year)  
<span style="color:gray;">: int // Unix timestamp of specific date</span>  
</summary>
<bdi class="fa">ساخت timestamp برای تاریخ مشخص</bdi>
<pre style="font-size: 12px">
$ts = mktime(10,30,0,9,14,2025);
echo date("Y-m-d H:i:s", $ts); // 2025-09-14 10:30:00
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
new Date(year: number, month: number, day?: number, hours?: number, minutes?: number, seconds?: number, ms?: number)  
<span style="color:gray;">: Date // specific date object</span>  
</summary>
<bdi class="fa">ایجاد شیء تاریخ مشخص</bdi>
<pre style="font-size: 12px">
let d = new Date(2025, 8, 14, 10, 30, 0); // month 0-based
console.log(d); // 2025-09-14T07:30:00.000Z
</pre></details>
## Format Date
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
date(string $format, int|null $timestamp = null)  
<span style="color:gray;">: string // formats timestamp</span>  
</summary>
<bdi class="fa">فرمت‌بندی تاریخ یا timestamp</bdi>
<pre style="font-size: 12px">
echo date("d/m/Y H:i"); // 14/09/2025 10:30
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
date.toLocaleString(locale?: string, options?: object)  
<span style="color:gray;">: string // formatted date string</span>  
</summary>
<bdi class="fa">فرمت‌بندی تاریخ با تنظیمات locale</bdi>
<pre style="font-size: 12px">
let d = new Date();
console.log(d.toLocaleString("en-US")); // 9/14/2025, 6:15:00 AM
</pre></details>
## Extract Parts of Date
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
date("Y", $timestamp) // year  
date("m", $timestamp) // month  
date("d", $timestamp) // day  
date("H", $timestamp) // hour  
date("i", $timestamp) // minute  
date("s", $timestamp) // second  
<span style="color:gray;">: string // specific part of date</span>  
</summary>
<bdi class="fa">استخراج بخش خاصی از تاریخ</bdi>
<pre style="font-size: 12px">
$ts = time();
echo date("Y-m-d H:i:s", $ts); // 2025-09-14 06:15:00
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
date.getFullYear() // year  
date.getMonth()    // month (0-11)  
date.getDate()     // day  
date.getHours()    // hour  
date.getMinutes()  // minute  
date.getSeconds()  // second  
<span style="color:gray;">: number // specific part of date</span>  
</summary>
<bdi class="fa">استخراج بخش خاصی از تاریخ</bdi>
<pre style="font-size: 12px">
let d = new Date();
console.log(d.getFullYear()); // 2025
console.log(d.getMonth()+1);  // 9
console.log(d.getDate());     // 14
</pre></details>
## Add / Subtract Time
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
strtotime(string $time, int|null $baseTimestamp = null)  
<span style="color:gray;">: int // timestamp after adding/subtracting relative time</span>  
</summary>
<bdi class="fa">اضافه یا کم کردن زمان نسبت به timestamp داده شده</bdi>
<pre style="font-size: 12px">
$ts = strtotime("+2 days");
echo date("Y-m-d", $ts); // 2025-09-16
</pre></details>
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
date.setFullYear(year: number, month?: number, date?: number)  
date.setMonth(month: number, day?: number)  
date.setDate(day: number)  
<span style="color:gray;">: number // sets specific part of date</span>  
</summary>
<bdi class="fa">اضافه یا کم کردن بخش خاصی از تاریخ</bdi>
<pre style="font-size: 12px">
let d = new Date();
d.setDate(d.getDate()+2); // add 2 days
console.log(d.toLocaleDateString());
</pre></details>
## Compare Dates
<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
$ts1 = strtotime("2025-09-14");  
$ts2 = strtotime("2025-09-16");  
<span style="color:gray;">: bool // compare timestamps</span>  
</summary>
<bdi class="fa">مقایسه تاریخ‌ها با timestamps</bdi>
<pre style="font-size: 12px">
if ($ts1 &lt; $ts2) echo "ts1 is earlier";
</pre></details>


<details dir="ltr"><summary style="overflow-x: auto;white-space: nowrap;">
date1.getTime() &lt; date2.getTime()  
<span style="color:gray;">: boolean // compare two Date objects</span>  
</summary>
<bdi class="fa">مقایسه دو شیء Date</bdi>
<pre style="font-size: 12px">
let d1 = new Date(2025,8,14);
let d2 = new Date(2025,8,16);
console.log(d1 &lt; d2); // true
</pre></details>

