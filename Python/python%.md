# مثال: حل معادله درجه دو
```python unwrap ln:false title=quadratic-equation.py
import math

# دریافت ضرایب از کاربر
a = float(input("Enter coefficient a: "))
b = float(input("Enter coefficient b: "))
c = float(input("Enter coefficient c: "))

""" -or- ``` : start of a multiLine_comment
if a == 0 then 
	معادله درجه دوم نیست (در واقع یک معادله خطی است)
"""
if a == 0:
    print("This is not a quadratic equation.")
             if b == 0 : print(“there is not any equation.”)
else:
	#محاسبه دلتای معادله                                                      
	delta = b**2 - 4*a*c

	#نمایش مقدار دلتا و بررسی نوع ریشه‌ها                     
	print(f"Delta ({chr(916)}) = {delta}")
	
	if delta > 0: # اگر دلتا بزرگ‌تر از صفر باشد، دو ریشه حقیقی و متمایز داریم
		x1 = (-b + math.sqrt(delta)) / (2*a) # محاسبه ریشه اول
		x2 = (-b - math.sqrt(delta)) / (2*a) # محاسبه ریشه دوم
		print("Two distinct real roots:")
		print(f"x1 = {x1}")  # چاپ ریشه اول
		print(f"x2 = {x2}")  # چاپ ریشه دوم
	elif delta == 0: # اگر دلتا دقیقاً صفر باشد، یک ریشه حقیقی (مضاعف) داریم
		x = -b / (2*a)  # ریشه مضاعف
		print("One real root (double root):")
		print(f"x = {x}")
	else: # اگر دلتا منفی باشد، ریشه‌ها موهومی هستند (عدد مختلط دارند)
		real_part = -b / (2*a) # قسمت حقیقی ریشه‌ها
		imag_part = math.sqrt(-delta) / (2*a) # قسمت موهومی ریشه‌ها
		print("No real roots. Two complex roots:")
		print(f"x1 = {real_part} + {imag_part}i") # چاپ ریشه اول موهومی
		print(f"x2 = {real_part} - {imag_part}i") # چاپ ریشه دوم موهومی
```
# مقدمات
|        subject        |                                                                 مثال                                                                 |
|:---------------------:|:------------------------------------------------------------------------------------------------------------------------------------:|
|      پسوند فایل       |                                                            myfilename.py                                                             |
|    inLine comments    |                                                              # comments                                                              |
|  multiLine comments   |                                  `''' multi-line-comments '''`<br><br>`""" multi-line-comments """`                                  |
|        blocks         |                                                     استفاده از دو نقطه و تورفتگی                                                     |
|   Case-Sensitivity    |                                                                 yes                                                                  |
| نشانه گذاری انتهای خط | مانند سایر زبان‌ها می‌توان از ; استفاده کرد اما عموما خالی می‌گذارند<br>با گذاشتن \ در انتهای خط می‌توان ادامه‌اش را در خط بعدی نوشت |
# متغیرها Variables
نام متغیرها نباید با عدد شروع شود. استفاده از کاراکترهای خاص مثل فاصله، نقطه، خط تیره، !، $ و @ نیز در متغیر مجاز نیست. بطور خلاصه  در نامگذاری متغیرها فقط می‌توان از کاراکترهای جدول زیر بصورت به هم چسبیده و بدون فاصله استفاده کرد:

| کاراکتر               | جواز                        |
| --------------------- | --------------------------- |
| حروف a تا z یا A تا Z | مجاز در هرجایی از نام متغیر |
| اعداد                 | در ابتدای متغیر نباید باشند |
| آندرلاین    _         | مجاز در هرجایی از نام متغیر |
# انتساب
```python ln:false
a = 10 #انتساب معمولی – عدم نیاز به تعیین نوع متغیر
a = 10; ...; a = “Hello” # تغییر چهرۀ یک متغیر در روند برنامه بصورت داینامیک
a, b, c, d = 11, ‘Ali’, false, 14 # انتساب همزان چندتایی و چند نوعی
a = b = c = 10; # انتساب همزمان یک مقدار به چند متغیر
a, b = b, a # جابجا کردن مقادیر
```
# محدودۀ دسترسی متغیرها - scopes

## اولویت دسترسی
مفهوم محدوده‌ی دسترسی (scope) بر اساس قانون LEGB عمل می‌کند. این قانون ترتیب جست‌وجوی متغیرهاست:

| level         | توضیح                                    |
| ------------- | ---------------------------------------- |
| L - Local     | درون همان تابع                           |
| E - Enclosing | تابع احاطه‌کننده (لایۀ بیرونی)           |
| G - Global    | در سطح فایل (global)                     |
| B - Built-in  | توابع پیش‌فرض پایتون مثل len() و print() |

## Function-Scope
متغیرهایی که داخل یک تابع تعریف می‌شوند Local هستند یعنی فقط درون همان تابع قابل استفاده هستند و از بیرون قابل دسترسی نیستند. متغیرهایی که خارج از تابع تعریف و ساخته شده باشند نیز در درون تابع ناشناخته و غیر قابل دسترس هستند. خلاصه اینکه توابع هیچ گونه نشتی چه از بیرون و چه به بیرون از خود ندارند و کاملا ایزوله و جدا هستند.

اگر بخواهیم درون تابع به یک متغیر سراسری دسترسی بیابیم باید با کلمه کلیدی global به تابع بگوییم که این متغیر خارجی است و باید در خارج از این تابع به دنبال آن بگردی:
```python ln=false
count = 0 # متغیر سراسری
def increase():
	global count # دسترسی به متغیر سراسری در درون یک تابع
	count += 1
```

در توابع تو در تو، متغیرهای لایه‌های بیرونی‌تر در لایه‌های داخلی‌تر قابل خواندن هستند اما قابل تغییر نیستند. برای تغییر باید با کلمه کلیدی nonlocal اعلام کنیم که در یک یا چند لایه بیرونی‌تر به دنبال آن بگردد.


|  موقعیت متغیر   | خواندن در تابع داخلی‌ | تغییر در تابع داخلی | روش تغییر در تابع داخلی |
| :-------------: | :-------------------: | :-----------------: | :---------------------: |
| در تابع بیرونی  |           ✅           |          ❌          |        nonlocal         |
| سراسری (global) |           ✅           |          ❌          |         global          |

توجه: اگر متغیر در global تعریف شده باشد آنگاه nonlocal به آن دسترسی ندارد بلکه باید از global استفاده کنیم.

## Class-scope
متغیرهایی که درون یک کلاس تعریف شده باشند آنگاه همیشه – چه در داخل کلاس و چه بیرون از آن – در هر جای برنامه به شکل className.variableName قابل دسترس هستند. البته برای راحتی کار فقط در درون کلاس می‌توان از کلمه کلیدی self به جای نام کلاس استفاده کرد: self.varibleName

متغیرهایی که بیرون از کلاس تعریف شده باشند در درون کلاس فقط بصورت read-only در دسترس هستند. اگر قصد تغییر آنها را داشته باشیم باید همانند توابع با کلمه کلیدی global بگوییم که این متغیر خارجی است.

## Global & other Scopes
متغیرهایی که خارج از توابع و کلاس‌ها تعریف می‌شوند، دارای محدوده سراسری یعنی global-scope هستند حتی اگر درون بلوک‌های حلقه و شرط و سایر بلوک‌ها باشند. این بدان معناست که در هرجای برنامه به جز توابع در دسترس هستند.

# ثابت‌ها

پایتون کلمه کلیدی خاصی برای ثابت‌ها ندارد، اما برای نشان دادن ثابت‌ها معمولاً از حروف بزرگ (uppercase) استفاده می‌شود:

PI = 3.14 # عدد پی

ثابتهای زیر در پایتون تعریف شده هستند:

|   |   |   |   |
|---|---|---|---|
|پی (π)|نپر (e)|بزرگ‌ترین مقدار (MAX Value)|کوچک‌ترین مقدار (MIN Value)|
|math.pi|math.e|sys.float_info.max|sys.float_info.min|

## Data Types

دستوری که نوع آرگومانش را برمی‌گرداند: type(expression)

Primitive Data Types داده‌های ابتدایی

|   |   |   |   |
|---|---|---|---|
|نوع داده (Type)|مثال (Example)|محدودیت مقدار یا طول|توضیح|
|int|42, -100|بی‌نهایت (محدود به RAM)|عدد صحیح بدون اعشار|
|float|3.14, -0.001|~1.8 x 10308|عدد اعشاری با دقت کمتر|
|bool|True, False|فقط دو مقدار|مقدار منطقی (درست یا نادرست)|
|str|"Hello"|عملاً بی‌نهایت|رشته متنی (کاراکترهای متوالی)|
|complex|2 + 3j|بی‌نهایت از نوع float|عدد مختلط (حقیقی و موهومی)|

Composite / Complex Data Types داده‌های مرکب / پیچیده

شامل list, tuple, set, dictionary که نگاه کنید به بخش Collections (Arrays).

# Operatorsعملگرها

**عملگرهای ریاضی Arithmetic**

عملیاتهای جمع و تفریق و ضرب و تقسیم در همه زبانها عبارتند از: + - * / . سایر عملگرها به قرار زیر هستند:

|   |   |   |   |   |
|---|---|---|---|---|
|عملگر|نام|مثال|نتیجه|توضیح|
|//|Floor Division (تقسیم کف)|7 // 3|2|عدد صحیح حاصل از تقسیم یعنی بدون قسمت اعشاری|
|%|Modulus (باقیمانده)|7 % 3|1|باقیمانده تقسیم ۷ بر ۳|
|**|Exponentiation (توان)|2 ** 3|8|توان: ۲ به توان ۳|

- تقسیم کف (Floor Division) دو عدد را بر هم تقسیم کرده و نتیجه را به پایین گرد می‌کند (به سمت منفی بی‌نهایت). مقدار اعشار را حذف می‌کند و فقط بخش صحیح باقی می‌ماند.

## عملگرهای مقایسه‌ای Comparison

مقایسۀ محتوا

|   |   |   |   |
|---|---|---|---|
|عملگر|توضیح فارسی|مثال|نتیجه مثال|
|==|مساوی است؟|5 == 5|True|
|!= یا <>|نامساوی است؟|5 != 3|True|
|>|بزرگ‌تر است؟|5 > 3|True|
|<|کوچک‌تر است؟|2 < 3|True|
|>=|بزرگ‌تر یا مساوی؟|5 >= 5|True|
|<=|کوچک‌تر یا مساوی؟|3 <= 5|True|

مقایسه هویت is, is not

|   |   |   |
|---|---|---|
|عملگر|نوع مقایسه|توضیح فارسی|
|==|مقایسه مقدار|بررسی می‌کند که مقدار دو شیء برابر است|
|!=|مقایسه مقدار|بررسی می‌کند که مقدارها برابر نیستند|
|is|مقایسه هویت (آدرس حافظه)|بررسی می‌کند که دو متغیر به همان شیء اشاره دارند|
|is not|مقایسه هویت (آدرس حافظه)|بررسی می‌کند که دو متغیر به شیء متفاوت اشاره دارند|

a = [1, 2, 3]; b = [1, 2, 3]; c = a

print(a == b) # ✅ True → مقدار یکسان

print(a is b) # ❌ False → اشیاء متفاوت

print(a is c) # ✅ True → همان شیء در حافظه

print(a != b) # ❌ False → مقدار برابر

print(a is not b) # ✅ True → اشیاء متفاوت

## عملگرهای منطقی Logical

|   |   |   |   |   |
|---|---|---|---|---|
|عملگر|نام|توضیح فارسی|مثال|نتیجه|
|and|AND|نتیجه True زمانی است که هر دو عبارت True باشند|True and False|False|
|or|OR|نتیجه True زمانی است که حداقل یکی از عبارت‌ها True باشد|True or False|True|
|not|NOT|معکوس نتیجه: اگر True باشد، False برمی‌گرداند و بالعکس|not True|False|

اتصال کوتاه (Short-Circuiting)

اتصال کوتاه به این معنی است که در هنگام ارزیابی یک عبارت منطقی با استفاده از عملگرهای and و or، اگر نتیجه از روی عملوند اول مشخص شود، عملوند دوم ارزیابی نمی‌شود. این ویژگی باعث افزایش کارایی و جلوگیری از ارزیابی غیرضروری می‌شود.

- عملگر and: اگر عملوند اول False باشد، نتیجه کلی همیشه False خواهد بود، بنابراین عملوند دوم ارزیابی نمی‌شود.
- عملگر or: اگر عملوند اول True باشد، نتیجه کلی همیشه True خواهد بود، بنابراین عملوند دوم ارزیابی نمی‌شود.

اولویت

عملگر and اولویت بالاتری نسبت به or دارد.

## عملگرهای انتساب Assignment

|   |   |   |   |
|---|---|---|---|
|عملگر|مثال|معادل با|توضیح فارسی|
|=|x = 5|---|مقدار ۵ به متغیر x انتساب داده می‌شود|
|+=|x += 3|x = x + 3|۳ واحد به مقدار فعلی x اضافه می‌شود|
|-=|x -= 2|x = x - 2|۲ واحد از مقدار فعلی x کم می‌شود|
|*=|x *= 4|x = x * 4|مقدار x در ۴ ضرب می‌شود|
|/=|x /= 2|x = x / 2|مقدار x بر ۲ تقسیم می‌شود (نتیجه اعشاری است)|
|//=|x //= 2|x = x // 2|تقسیم صحیح (بدون اعشار)|
|%=|x %= 3|x = x % 3|باقیمانده تقسیم x بر ۳|
|**=|x **= 2|x = x ** 2|مقدار x به توان ۲ می‌رسد|

عملگرهای انتساب بیتی bitwise

|   |   |   |   |
|---|---|---|---|
|&=|x &= y|x = x & y|عملیات بیتی AND بین x و y|
|`|Bitwise OR Assignment|`x|= y`|
|^=|x ^= y|x = x ^ y|عملیات بیتی XOR بین x و y|
|<<=|x <<= 1|x = x << 1|شیفت بیتی به چپ (ضرب در ۲)|
|>>=|x >>= 1|x = x >> 1|شیفت بیتی به راست (تقسیم بر ۲)|

## عملگرهای الحاق رشته

|   |   |   |   |   |
|---|---|---|---|---|
|عملگر|نام|مثال|نتیجه|توضیح|
|+|الحاق رشته|"Hello" + " World"|"Hello World"|دو رشته را به هم متصل می‌کند|
|+=|انتساب با الحاق|s = "Hi"; s += "!"|"Hi!"|رشته‌ی "!" به انتهای s اضافه می‌شود|

## عملگرهای unpacking

عملگرهای * و ** وقتی پشت یک لیست، تاپل، ست، دیکشنری یا رشته قرار بگیرند آنگاه آن را به عناصر و اعضاء یا به کاراکترهای آن باز می‌کند. دو ستاره فقط برای دیکشنری به کار می‌رود. تک ستاره فقط با داده‌های iterable مثل str, list, tuple, set و غیره کار می‌کند. هر چیزی که بشود روی آن for زد، می‌شود با * بازش کرد.

word = "Hello"; print(*word) # H e l l o

list1 = [1, 2, 3, 4]; list2 = [5, 6]

print(*list1) # 1 2 3 4

print(list1) # [1, 2, 3, 4]

newList = [*list1, *list2] # لیست جدید = [ بازشده ی لیست اول , باز شده ی لیست دوم ]

print(*newList) # 1 2 3 4 5 6 نمایش باز شده ی لیست جدید

dict1 = {'a': 1, 'b': 2}

dict2 = {'b': 3, 'c': 4}

newDict = {**dict1, **dict2} # دیکشنری جدید = { بازشدۀ دیکشنری اول , باز شدۀ دیکشنری دوم }

print(newDict) # خروجی: {'a': 1, 'b': 3, 'c': 4}

## اولویت اجرای عملگرها

اگر پرانتز باشد همیشه اولویت اول با پرانتز است. بالاترین اولیت با پرانتز است و کمترین اولویت با عملگرهای انتساب(=, +=, -=, /=, %=, //=, *=, **=) است. همیشه ابتدا عملگرهای با اولویت بالاتر ارزیابی می‌شوند، و سپس مقدار نهایی به متغیر انتساب داده می‌شود. ترتیب اولویت محاسبه در سایر عملگرهای به قرار زیر است:

- عملگرهای ریاضی: اول توان، دوم ضرب(*) و تقسیم(/, //, %)، سوم جمع و تفریق.
- عملگرهای مقایسه‌ای: اول مساوی و نامساوی(==, !!=, <>)، دوم بزرگتر و کوچکتر(>, >=, <, <=)، سوم مقایسه هویت
- عملگرهای عضویت : in, in not
- عملرهای منطقی : اول not، دوم and، سوم or

# Input / Output

مقداری که یک عبارت برمی‌گرداند را می‌توانیم به عنوان خروجی به جاهای مختلفی بفرستیم که برخی از آنها عبارتند از:

|   |   |
|---|---|
|نوع خروجی|توضیح|
|ترمینال (Standard Output - stdout)|نمایش مستقیم خروجی در صفحهٔ ترمینال print("Hello")|
|فایل|ذخیره خروجی در فایل متنی، CSV، JSON، log و غیره|
|خروجی خطا (Standard Error - stderr)|ارسال پیام‌های خطا یا هشدار|
|شبکه / API|ارسال داده به سرور یا دریافت از API|
|پایگاه داده (Database)|ذخیره خروجی در پایگاه داده: SQLite، MySQL، PostgreSQL|
|GUI (رابط گرافیکی کاربر)|نمایش خروجی در پنجره‌های گرافیکی با استفاده ازkinter و غیره|
|وب‌سایت (HTML Output)|تولید خروجی برای صفحات وب یا خروجی HTML مستقیم|
|گراف‌ها و نمودارها|نمایش داده‌ها به‌صورت تصویری (نمودار، پلات و...)|
|دستگاه‌ها / حسگرها|فرستادن داده به دستگاه فیزیکی، مثلاً Arduino|
|کنسول‌های IDE (مثل VS Code, PyCharm)|مثل ترمینال، ولی داخل محیط کدنویسی|

اگر بخواهیم مقداری که یک عبارت برمی‌گرداند را به عنوان خروجی به ترمینال بفرستیم تا در آنجا نمایش داده شود آنگاه از دستور print در پایتون استفاده می‌کنیم:

print(expressions,,,)

print(*objects,,, sep=' ', end='\n', file=sys.stdout, flush=False)

- objects: تمام آرگومان‌هایی که در تابع print به‌طور پیش‌فرض ارسال می‌شوند.
- sep: تعیین جداکننده بین آیتم‌های چاپ شده. پیشفرض یک فاصله است.
- end: مشخص می‌کند که در انتهای چاپ آیتم‌ها، چه چیزی قرار داده شود. پیش‌فرض یک خط جدید \n است و بنابراین اگر این پارامتر ذکر نشود، دستور print بعدی در خطی جدید نمایش داده خواهد شد. برای جلوگیری از این کار و حفظ پیوستگی از end=’’ یا end=’somethings’ استفاده می‌شود.
- file: تعیین می‌کند که خروجی به کجا ارسال شود (به‌طور پیش‌فرض به ترمینال یا stdout است).
- flush: پیش‌فرض این است که خروجی ممکن است در بافر (buffer) ذخیره شود و تا زمانی که لازم نباشد، به صورت فوری چاپ نشود. اما اگر True باشد، بلافاصله داده‌ها به خروجی ارسال می‌شوند. یعنی پایتون بلافاصله بافر خروجی را پاک می‌کند و همه‌ی چیزهایی که قرار است چاپ شوند، فوراً نمایش داده می‌شوند. موارد کاربرد: نمایش مرحله به مرحله پیشرفت در برنامه‌های دارای progress bar / فوراً نشان دادن پیغام‌ها به کاربر در برنامه‌های تعاملی یا real-time / جلوگیری از از‌دست‌رفتن اطلاعات در صورت قطع برنامه و نوشتن در فایل‌های لاگ.

print("Python", "is", "awesome", sep=" 🔹 ", end="!\n")

# Python 🔹 is 🔹 awesome!

# هدایت خروجی به نوشتن در فایل و نه ترمینال

f = open("output.txt", "w", encoding="utf-8")

print("این یک خروجی بدواین خروجی درون فایل نوشته میشود و نه در ترمینال", file=f)

print("Python", "rocks!", sep=" ❤️ ", file=f)

f.close()

# یا به صورت زیر

with open("output.txt", "w", encoding="utf-8") as f:

print("این یک متن نمونه است.", file=f)

print("Python", "is", "awesome", sep=" 🔹 ", end="!\n", file=f)

## input

```python ln:false
input(prompt) #->string
x, y, z = map( int, input("سه عدد را با فاصله وارد کن :").split() ) # مثال
# سه عدد اگر با فاصله وارد شوند آنگاه متد اسپلیت آنها را بر اساس فاصله تشخیص و جداسازی و تبدیل به لیست می‌کند. در ابتدا اعضاء این لیست، رشته هستند اما در نهایت، خروجی یک لیست شامل اعضایی است که متد اینتجر روی تک تک آنها مَپ شده و همگی تبدیل به عدد شده‌اند.
```

# ساختارهای شرطی و تصمیم

شرط با if

if condition : codes

elif condition : codes ®

else: codes

انتساب شرطی

result = value_if_true if condition else value_if_false

شرط چند مسیره - سوئیچینگ

match statement :

case someReturn : codes ®

case _: codes

# ساختارهای تکرار

## حلقه for

```python ln=false
for ~ in ~ : 
	<codes>
else :
	# loop-finished-successfully-codes کدهای اتمام عادی حلقه و نه اجباری
	print("Loop finished successfully.")
```
---
```python ln=false
for <item> in <collection> : <codes>
for    _   in <collection> : <codes>
	# آندرلاین زمانیکه متغیر پیمایشی در درون حلقه کاربردی ندارد
for item in [~, ~, ... ~] : codes
for item in range(start=0 , non-inclusive-endpoint , step=1):codes
for key|val|key,val in dict|dict.values()|dict.items() : codes
# تکرار روی کلیدها، مقادیر، هردو
for index,item in enumerate(list): codes تکرار روی اندیس‌ها و مقادیر در یک لیست
for char in text : codes رشته
for index, char in enumerate(text): codes رشته
for i in range(len(text)): codes ~text[i] رشته
```

## حلقه While

```python ln:false
while booleanStatement : codes
```

## تغییر جریان حلقه

{python} break

#usage: for Loops, while Loops, اتمام اجباری و خروج از حلقه (در حلقه‌های تو دو تو فقط خروج از یک لایه)
{python} Continue # پرش به حلقۀ بعدی

# Truthy / Falsy

Truthy و Falsy به مقادیری اشاره دارند که در یک زمینه شرطی (Boolean Context) به ترتیب به True یا False تبدیل می‌شوند.

- Falsy Values، مقادیری هستند که در در یک زمینه شرطی مانند if (x) به عنوان False ارزیابی می‌شوند.
- Truthy Values، مقادیری هستند که در در یک زمینه شرطی مانند if (x) به عنوان True ارزیابی می‌شوند.

|   |   |
|---|---|
|Truthy Values|Falsy Values|
|True<br><br>Non-zero numbers,<br><br>non-empty strings,<br><br>non-empty containers|False<br><br>0<br><br>“”<br><br>[], {}, (), set(), range(0)<br><br>None|

# توابع و کلاس ها

## توابع

def myfunc(args,,,): codes... return something ریترن باعث خروج از تابع می‌شود

توابع بی‌نام

result = lambda arguments : expression

## کلاس‌ها و اشیاء

class myclass(parentClass):

|pass # no code اگر بخواهیم درون کلاس هیچ کدی ننویسیم(کلاس خالی)

|def __init__(self,args,,,): assignments as self.arg1=arg1

|def __str__(self):return f"..." #print(),str() هنگام فراخوانی ابجکت با توابع روبرو

codes; myprop=xxx; def mymethod(args):xxx;

obj = myclass() # ~obj.myprop; ~obj.mymetho(args)

# Collections (Arrays)

a=[ “hello”, 2.5, 3, [5,10] ]; b=[]

## properties

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|Type|Ordered|Changeable<br><br>تغییر در اعضاء|Duplicatable<br><br>اعضاء تکراری|Indexed|Notes|
|List|✅ Yes|✅ Yes|✅ Yes|✅ Yes|پراستفاده‌ترین؛ مثل آرایه‌های پویاست|
|Tuple|✅ Yes|❌ No|✅ Yes|✅ Yes|غیرقابل‌تغییر؛ از لیست سریع‌تر است|
|Set|❌ No|❌ No*|❌ No|❌ No|فقط اعضای یکتا؛ frozenset|
|Dictionary|✅ Yes**|✅ Yes|❌ No|✅ Keys|دوتایی‌های Key-value|

- در یک مجموعه مرتب (Ordered) مثل list، ترتیب ورود اعضاء حفظ می‌شود. در ست‌ها این ترتیب حفظ نمی‌شود.
- Indexed یعنی می‌توانیم به اعضای مجموعه با استفاده از شماره یا کلید دسترسی پیدا کنیم.
- ست ها و تاپل ها غیر قابل تغییر و ثابت هستند.

Sets

ست‌ها (Set) قابل‌تغییر (mutable) هستند، برای ایجاد یک ست غیرقابل‌تغییر از frozenset استفاده می‌شود. ست‌ها برای جلوگیری از وجود تکرار طراحی شده‌اند. تنها بر اساس محتوا و یونیک بودن اعضا تمرکز دارند، نه ترتیب. در ست‌ها ترتیب حفظ نمی‌شود و بنابراین قابل ایندکس هم نیست.

Tuples

تاپل‌ها پس از ایجاد، قابل تغییر نیستند. یعنی نمی‌توان اعضای آن را اضافه، حذف یا تغییر داد. چون تاپل‌ها غیرقابل تغییر هستند، اعضای آن در زمان ایجاد ثابت هستند و نمی‌توان به آنها عناصر جدیدی اضافه کرد یا آنها را حذف کرد. چون تاپل‌ها غیرقابل تغییر هستند، پایتون می‌تواند آنها را سریع‌تر از لیست‌ها پردازش کند. این ویژگی باعث می‌شود تاپل‌ها برای ذخیره داده‌های ثابت و دسترسی به آنها در الگوریتم‌های زمان-حساس مناسب باشند. چون تاپل‌ها غیرقابل تغییر هستند، می‌توان از آنها به عنوان کلید در دیکشنری‌ها استفاده کرد، در حالی که لیست‌ها نمی‌توانند کلید باشند.

- ثابت نگه‌داشتن داده‌ها: برای داده‌هایی که نباید تغییر کنند.
- استفاده به عنوان کلید دیکشنری: اگر می‌خواهید از مجموعه‌ای از مقادیر به عنوان کلید در دیکشنری استفاده کنید.
- انتقال داده‌های چندگانه: برای بازگرداندن چندین مقدار از یک تابع، چون تاپل‌ها می‌توانند مقادیر مختلف را با هم نگه دارند.

## Work with collections

Definition

|   |   |   |
|---|---|---|
|Collection Type||Syntax & Example|
|List|list_name = [item1, item2, ...]|my_list = [1, 2, 3]<br><br>my_list = list((1, 2, 3))|
|Tuple|tuple_name = (item1, item2, ...)|my_tuple = (1, 2, 3)|
|Set|set_name = {item1, item2, ...}|my_set = {1, 2, 3}|
|Dictionary|dict_name = {key1: value1, key2: value2, ...}|my_dict = {'a': 1, 'b': 2}|

Index

- ایندکس می‌تواند یک عبارت محاسبه‌ای expression یا متغیر باشد.
- نوع ایندکس حتما باید int یعنی عددی صحیح باشد. ( index=2.0 : error)
- ایندکس 0 اشاره به اولین عنصر و ایندکس -1 اشاره به آخرین عنصر دارد.
- ایندکس منفی از انتهای مجموعه می‌شمارد.
- در مجموعه‌های تو در تو از دو یا چند ایندکس استفاده می‌شود:

myMatrix = [ [1,2,3], [4,5,6], [7,8,9] ]

myMatrix [2][0] -> 4

Length | count

|   |   |   |
|---|---|---|
|Collection Type|Len(collection-name)|#.count(x)|
|List|len(my_list) → 4|my_list.count(1) → 1|
|Tuple|len(my_tuple) → 3|my_tuple.count(2) → 1|
|Set|len(my_set) → 3|❌|
|Dictionary|len(my_dict) → 2|list(my_dict.values()).count(1) → 1|

- Len(): این تابع تعداد کل عناصر در یک کالکشن را برمی‌گرداند.
- Count(): این تابع تعداد دفعاتی که یک عنصر خاص در کالکشن ظاهر می‌شود را برمی‌گرداند.

Access Items

|   |   |   |
|---|---|---|
|Collection Type||Syntax & Example|
|List|list_name[index]|my_list[0] → 1|
|Tuple|tuple_name[index]|my_tuple[1] → 2|
|Set||❌ Not indexed|
|Dictionary|dict_name[key]|my_dict['key1'] → value1|

Change / Update Items

|   |   |   |
|---|---|---|
|Collection Type||Syntax & Example|
|List|list_name[index] = new_value|my_list[0] = 10|
|Tuple||❌ Immutable (cannot modify)|
|Set||❌ No direct item update|
|Dictionary|dict_name[key] = new_value|my_dict['key1'] = newValue|

Add Items

|   |   |   |
|---|---|---|
|Collection Type||Syntax & Example|
|List|list_name.append(item)|my_list.append(4)|
|Tuple||❌ Not allowed (immutable)|
|Set|set_name.add(item)|my_set.add(4)|
|Dictionary|dict_name[new_key] = value|my_dict['newKey'] = value|

Remove Items

|   |   |   |
|---|---|---|
|Collection Type||Syntax & Example|
|List|list_name.remove(item)|my_list.remove(2)|
|Tuple||❌ Not allowed|
|Set|set_name.remove(item)|my_set.remove(2)|
|Dictionary|del dict_name[key]|del my_dict['someKey']|

Loop on Items

|   |   |
|---|---|
|Collection Type|Syntax & Example|
|List|for item in my_list:|
|Tuple|for item in my_tuple:|
|Set|for item in my_set:|
|Dictionary|for key in my_dict: codes<br><br>for value in my_dict.values() : codes<br><br>for key, value in my_dict.items(): codes|

Sort Items

|   |   |
|---|---|
|Collection Type|Syntax & Example|
|List|my_list.sort()|
|Tuple|sorted(my_tuple) → returns list|
|Set|sorted(my_set) → returns list|
|Dictionary|sorted(my_dict) → returns sorted keys|

Copy Collection

|   |   |
|---|---|
|Collection Type|Syntax & Example|
|List|new_list = my_list.copy()<br><br>new_list = my_list[:]|
|Tuple|new_tuple = my_tuple[:]|
|Set|new_set = my_set.copy()|
|Dictionary|new_dict = my_dict.copy()|

Join Collections

|   |   |
|---|---|
|Collection Type|Syntax & Example|
|List|combined = my_list1 + my_list2|
|Tuple|combined = my_tuple1 + my_tuple2|
|Set|combined = my_set1.union(my_set2)|
|Dictionary|combined = {**dict1, **dict2} (Python 3.5+)|

## Methods

🟦 Add/Append Items

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|Method|List|Tuple|Set|Dic|Description|
|.append(x)|✅|❌|❌|❌|یک عنصر را به انتهای مجموعه اضافه می‌کند.|
|.extend(iterable)|✅|❌|❌|❌|تمام عناصر از یک iterable را به مجموعه اضافه می‌کند.|
|.add(x)|❌|❌|✅|❌|یک عنصر را به مجموعه اضافه می‌کند.|
|.update(iterable)|❌|❌|✅|✅|چندین عنصر از یک iterable را به مجموعه اضافه می‌کند.|

🟦Remove/Clear Items

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|Method|List|Tuple|Set|Dic|Description|
|.remove(x)|✅|❌|✅|❌|اولین وقوع عنصر x را حذف می‌کند.|
|.pop([index])|✅|❌|✅|✅|یک آیتم را حذف کرده و آن را برمی‌گرداند (با اندیس اختیاری).|
|.clear()|✅|❌|✅|✅|تمام آیتم‌ها را از مجموعه حذف می‌کند.|
|.discard(x)|❌|❌|✅|❌|اگر عنصر وجود داشته باشد آن را حذف می‌کند، در غیر این صورت هیچ کاری نمی‌کند.|
|del key|❌|❌|❌|✅|یک آیتم را بر اساس کلید آن حذف می‌کند.|

🟦Update Items

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|Method|List|Tuple|Set|Dic|Description|
|.insert(index, x)|✅|❌|❌|❌|یک عنصر را در اندیس مشخص شده وارد می‌کند.|
|.setdefault(key, default)|❌|❌|❌|✅|اگر کلید موجود نباشد، مقدار پیش‌فرض را تنظیم می‌کند.|
|.update(dict)|❌|❌|❌|✅|دیکشنری را با آیتم‌های یک دیکشنری دیگر به‌روز می‌کند.|

🟦Access Items

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|Method|List|Tuple|Set|Dic|Description|
|.index(x)|✅|✅|❌|❌|اندیس اولین وقوع عنصر x را برمی‌گرداند.|
|.count(x)|✅|✅|❌|❌|تعداد وقوع‌های عنصر x را برمی‌گرداند.|
|.get(key)|❌|❌|❌|✅|مقدار یک کلید را برمی‌گرداند یا None اگر پیدا نشد.|
|.keys()|❌|❌|❌|✅|تمام کلیدهای دیکشنری را به‌صورت نمایشی برمی‌گرداند.|
|.values()|❌|❌|❌|✅|تمام مقادیر دیکشنری را به‌صورت نمایشی برمی‌گرداند.|
|.items()|❌|❌|❌|✅|تمام جفت‌های کلید-مقدار دیکشنری را به‌صورت نمایشی برمی‌گرداند.|

🟦Sort/Reverse

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|Method|List|Tuple|Set|Dic|Description|
|.sort()|✅|❌|❌|❌|لیست را در محل مرتب می‌کند.|
|.reverse()|✅|❌|❌|❌|ترتیب عناصر لیست را معکوس می‌کند.|

🟦Copy Collection

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|Method|List|Tuple|Set|Dic|Description|
|.copy()|✅|❌ (use slicing)|✅|✅|یک کپی سطحی از مجموعه برمی‌گرداند.|

🟦Join Collections

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|Method|List|Tuple|Set|Dic|Description|
|+|✅|✅|❌|❌|مجموعه‌ها را ترکیب و اتصال می‌دهد.|
|.union(set)|❌|❌|✅|❌|دو مجموعه ست را ترکیب کرده و تکراری‌ها را حذف می‌کند (یکتاسازی)|
|** (merge)|❌|❌|❌|✅|دو دیکشنری را به یکدیگر ترکیب می‌کند.|

🟦Difference & Intersection

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|Method|List|Tuple|Set|Dic|Description|
|.difference(set)|❌|❌|✅|❌|عناصر موجود در یک مجموعه و نه دیگری را برمی‌گرداند.|
|.intersection(set)|❌|❌|✅|❌|اشتراک دو مجموعه ست را برمی‌گرداند (عناصر مشترک).|

🟦Additional Methods

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|Method|List|Tuple|Set|Dic|Description|
|len()|✅|✅|✅|✅|تعداد عناصر موجود در مجموعه را برمی‌گرداند.|
|.pop()|✅|❌|✅|✅|آخرین آیتم یا یک عنصر تصادفی را حذف کرده و برمی‌گرداند.|
|.popitem()|❌|❌|❌|✅|یک جفت کلید-مقدار را از دیکشنری حذف کرده و برمی‌گرداند.|
|.fromkeys(keys, value)|❌|❌|❌|✅|یک دیکشنری از کلیدها با یک مقدار مشترک ایجاد می‌کند.|

## Operators

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|Opr.|List|Tuple|Set|Dic|Description|
|+|✅|✅|❌|❌|دو مجموعه از یک نوع را ترکیب می‌کند<br><br>[1, 2] + [3, 4] → [1, 2, 3, 4]; 'Hello' + 'World' → 'HelloWorld'|
|*|✅|✅|❌|❌|مجموعه را به تعداد مشخصی تکرار می‌کند.<br><br>[1, 2] * 2 → [1, 2, 1, 2]; 'Hi' * 3 → 'HiHiHi'|
|in|✅|✅|✅|✅|بررسی که آیا یک عنصر در مجموعه یا یک کلید در دیکشنری وجود دارد.(عضویت)<br><br>3 in [1, 2, 3] results in True; 'at' in 'cat' → True|
|not in|✅|✅|✅|✅|آیا یک عنصر در مجموعه وجود ندارد یا یک کلید در دیکشنری وجود ندارد.<br><br>4 not in [1, 2, 3] → True; 'x' not in 'cat' → True|
|==|✅|✅|✅|✅|دو مجموعه را برای برابری مقایسه می‌کند.<br><br>[1, 2] == [1, 2] → True; 'abc' == 'abc' → True|
|!=|✅|✅|✅|✅|بررسی می‌کند که آیا دو مجموعه برابر نیستند.<br><br>[1, 2] != [2, 3] → True; 'abc' != 'abd' → True<br><br>'abc' != 'abd' → True ; 'cat' > 'car' → True|
|<, <=<br><br>>, >=|✅|✅|❌|❌|مجموعه‌ها را به صورت لغوی (عنصر به عنصر) مقایسه می‌کند.<br><br>[1, 2] < [2, 3] → True; 'abc' < 'bcd' → True|
|&|❌|❌|✅|❌|عناصر مشترک بین دو مجموعه ست را پیدا می‌کند.<br><br>{1, 2} & {2, 3} → {2}|
|`|`|❌|❌|✅|دو مجموعه ست را ترکیب می‌کند و تکراری‌ها را حذف می‌کند.|
|-|❌|❌|✅|❌|تفاوت بین دو مجموعه ست را پیدا می‌کند.<br><br>{1, 2, 3} - {2, 3, 4} → {1}|
|^|❌|❌|✅|❌|عناصری که در هر یک از مجموعه‌ها هستند ولی در هر دو نیستند را برمی‌گرداند.<br><br>{1, 2} ^ {2, 3} → {1, 3}|
|[]|✅|✅|❌|✅|یک بازه از عناصر را بازیابی می‌کند<br><br>[1, 2, 3, 4][1:3] → [2, 3]; 'Hello'[1:4] → 'ell'|

# Built-in Functions

Logical Checks

|   |   |   |
|---|---|---|
|Function|Description|Example|
|all()|True if all True|all([True, True]) → True|
|any()|True if any True|any([0, 1]) → True|
|callable()|Checks callability|callable(len) → True|
|isinstance()|Checks instance|isinstance(5, int) → True|
|issubclass()|Checks subclass|issubclass(bool, int) → True|
|hasattr()|Checks for attribute|hasattr('abc', 'upper')|
|getattr()|Gets attribute|getattr(str, 'upper')('abc')|
|setattr()|Sets attribute|setattr(obj, 'x', 5)|
|delattr()|Deletes attribute|delattr(obj, 'x')|

Method & Class Utilities

|   |   |   |
|---|---|---|
|Function|Description|Example|
|classmethod()|Class method decorator|@classmethod|
|staticmethod()|Static method decorator|@staticmethod|

🟦 Numeric & Math

|   |   |   |
|---|---|---|
|Function|Description (فارسی)|Example|
|abs()|مقدار مطلق عدد را برمی‌گرداند|abs(-5) → 5|
|bin()|عدد را به رشته دودویی تبدیل می‌کند|bin(5) → '0b101'|
|complex()|یک عدد مختلط ایجاد می‌کند|complex(1, 2) → (1+2j)|
|divmod()|خارج‌قسمت و باقیمانده را همزمان برمی‌گرداند|divmod(8, 3) → (2, 2)|
|float()|عدد را به عدد اعشاری تبدیل می‌کند|float(3) → 3.0|
|hex()|عدد را به مبنای شانزده تبدیل می‌کند|hex(255) → '0xff'|
|int()|عدد یا رشته را به عدد صحیح تبدیل می‌کند|int("10") → 10|
|oct()|عدد را به مبنای هشت تبدیل می‌کند|oct(8) → '0o10'|
|pow()|توان یک عدد را محاسبه می‌کند|pow(2, 3) → 8|
|round()|عدد اعشاری را گرد می‌کند|round(3.6) → 4|
|sum()|مجموع مقادیر یک کالکشن را محاسبه می‌کند|sum([1, 2, 3]) → 6|
|max()|بزرگ‌ترین مقدار را از یک کالکشن برمی‌گرداند|max([1, 5, 2]) → 5|
|min()|کوچک‌ترین مقدار را از یک کالکشن برمی‌گرداند|min([1, 5, 2]) → 1|

🟦 Sequence & Iterable

|   |   |   |
|---|---|---|
|Function|Description (فارسی)|Example|
|len()|تعداد آیتم‌های موجود در یک شی قابل شمارش را برمی‌گرداند|len("abc") → 3|
|enumerate()|یک شمارنده به آیتم‌های iterable اضافه می‌کند|enumerate(['a','b'])|
|range()|یک دنباله از اعداد تولید می‌کند|range(5) → [0,1,2,3,4]|
|reversed()|ترتیب آیتم‌ها را معکوس می‌کند|list(reversed([1,2,3]))|
|slice()|یک شی برش برای برش دادن سکوئنس‌ها ایجاد می‌کند|slice(1, 3)|
|zip()|چند iterable را به صورت زوج‌های تکراری ترکیب می‌کند|zip([1,2],[3,4])|
|sorted()|کالکشن را مرتب‌شده برمی‌گرداند|sorted([3,1,2]) → [1,2,3]|
|next()|Next item of iterator|next(iter([1,2]))|
|iter()|Returns iterator|iter([1,2,3])|

Functional Programming

|   |   |   |
|---|---|---|
|Function|Description|Example|
|map()|Applies function|map(str.upper, ['a','b'])|
|filter()|Filters values|filter(str.isalpha, 'abc123')|

🟦 Type Conversion & Construction

|   |   |   |
|---|---|---|
|Function|Description|Example|
|int()|Converts to integer|int(5.8) → 5|
|float()|Converts to float|float(3) → 3.0|
|complex()|Converts to complex|complex(1, 2) → (1+2j)|
|str()|Converts to string|str(100) → '100'|
|bool()|Converts to boolean|bool(0) → False|
|ascii()|Escapes non-ASCII chars|ascii('ä') → '\\xe4'|
|repr()|Returns code-like string|repr('abc') → "'abc'"|
|format()|Formats values|format(255, 'x') → 'ff'|
|bin()|Converts to binary|bin(10) → '0b1010'|
|hex()|Converts to hexadecimal|hex(255) → '0xff'|
|oct()|Converts to octal|oct(8) → '0o10'|
|ord()|Unicode of character|ord('A') → 65|
|chr()|Character from Unicode|chr(65) → 'A'|
|list()|یک لیست از یک iterable می‌سازد|list("abc") → ['a','b','c']|
|tuple()|یک تاپل از یک iterable می‌سازد|tuple([1,2]) → (1, 2)|
|set()|یک مجموعه از آیتم‌های غیرتکراری می‌سازد|set([1,2,2]) → {1,2}|
|dict()|یک دیکشنری ایجاد می‌کند|dict(a=1, b=2)|
|frozenset()|مجموعه‌ای غیرقابل تغییر ایجاد می‌کند|frozenset([1,2,3])|
|memoryview()|دسترسی سطح پایین به داده‌های باینری فراهم می‌کند|memoryview(b"abc")|
|bytes()|Creates bytes object|bytes('abc', 'utf-8')|
|bytearray()|Mutable byte array|bytearray('abc', 'utf-8')|
|memoryview()|Memory view of bytes|memoryview(b'abc')|

🟦 Object & Class & Attribute

|   |   |   |
|---|---|---|
|Function|Description (فارسی)|Example|
|type()|نوع شی را برمی‌گرداند|type(5) → <class 'int'>|
|isinstance()|بررسی می‌کند آیا شی از نوع خاصی است|isinstance(5, int) → True|
|issubclass()|بررسی می‌کند آیا یک کلاس زیرکلاس کلاس دیگر است|issubclass(bool, int)|
|getattr()|مقدار ویژگی شی را می‌گیرد|getattr(obj, 'x')|
|setattr()|مقدار ویژگی شی را تنظیم می‌کند|setattr(obj, 'x', 10)|
|delattr()|ویژگی شی را حذف می‌کند|delattr(obj, 'x')|
|hasattr()|بررسی می‌کند آیا شی ویژگی مشخصی دارد|hasattr(obj, 'x')|
|property()|تعریف ویژگی‌های قابل کنترل در کلاس|Used with class attributes|
|object()|شی جدید پایه ایجاد می‌کند|object()|
|classmethod()|متدی را به متد کلاسی تبدیل می‌کند|Used in class definitions|
|staticmethod()|متدی را بدون وابستگی به کلاس یا شی تعریف می‌کند|Used in class definitions|
|super()|به متدهای کلاس پدر دسترسی می‌دهد|super().method()|

🟦 Functional Programming

|   |   |   |
|---|---|---|
|Function|Description (فارسی)|Example|
|filter()|آیتم‌های خاصی از iterable را براساس شرط فیلتر می‌کند|filter(str.isalpha, 'abc123')|
|map()|یک تابع را به همه آیتم‌های iterable اعمال می‌کند|map(str.upper, 'abc')|
|all()|بررسی می‌کند آیا همه آیتم‌ها درست هستند|all([True, True]) → True|
|any()|بررسی می‌کند آیا حداقل یکی از آیتم‌ها درست است|any([False, True]) → True|

🟦 Input/Output & Formatting

|   |   |   |
|---|---|---|
|Function|Description (فارسی)|Example|
|input()|ورودی کاربر را دریافت می‌کند|input("Name: ")|
|print()|داده‌ها را در خروجی استاندارد چاپ می‌کند|print("Hello")|
|format()|مقدار را به صورت قالب‌بندی شده برمی‌گرداند|"{} {}".format('Hi','Bob')|
|ascii()|نسخه خوانا از شی ارائه می‌دهد و کاراکترهای غیر ASCII را فرار می‌دهد|ascii("تست")|
|repr()|نمایش رسمی و قابل بازتولید شی را برمی‌گرداند|repr("Hello")|

🟦 Evaluation & Execution

|   |   |   |
|---|---|---|
|Function|Description (فارسی)|Example|
|eval()|یک عبارت را ارزیابی و اجرا می‌کند|eval("2+2") → 4|
|exec()|یک رشته کد پایتون را اجرا می‌کند|exec("x=5")|
|compile()|کد را به شیء قابل اجرا تبدیل می‌کند|compile("2+2", '', 'eval')|

🟦 Introspection & Debug

|   |   |   |
|---|---|---|
|Function|Description (فارسی)|Example|
|dir()|ویژگی‌ها و متدهای شی را لیست می‌کند|dir(str)|
|vars()|ویژگی‌های شی را به صورت دیکشنری برمی‌گرداند|vars(obj)|
|globals()|جدول نمادهای سراسری را برمی‌گرداند|globals()|
|locals()|جدول نمادهای محلی را برمی‌گرداند|locals()|
|help()|سیستم راهنمای داخلی را اجرا می‌کند|help(str)|
|Function|Description|Example|
|id()|Object ID|id(3)|
|property()|Creates property|property(get_x)|
|super()|Parent class|super().method()|
|object()|Creates object|object()|
|hash()|Hash value|hash('abc')|

Others

|   |   |   |
|---|---|---|
|Function|Example|Description (فارسی)|
|id()|id(5)|آدرس حافظه شی را برمی‌گرداند|
|hash()|hash("a")|مقدار هش شی را برمی‌گرداند|
|callable()|callable(len) → True|بررسی می‌کند آیا شی قابل فراخوانی است|
|chr()|chr(65) → 'A'|کد یونیکد را به کاراکتر تبدیل می‌کند|
|ord()|ord('A') → 65|کاراکتر را به کد یونیکد تبدیل می‌کند|

File Handling

|   |   |   |
|---|---|---|
|open()|open('file.txt')|فایل را باز می‌کند و شی فایل برمی‌گرداند|

# math functions

در مواردیکه از math.~ استفاده شده است باید کتابخانه‌اش import گردد: import math

توابع پایه (Basic Math Functions)

|   |   |   |   |
|---|---|---|---|
|تابع|کاربرد|مثال|خروجی|
|abs(x)|قدر مطلق|abs(-5)|5|
|round(x)|گرد کردن عدد|round(3.6)|4|
|round(x, n)|گرد کردن تا n رقم اعشار|round(3.14159, 2)|3.14|
|max(a, b, ...)|بیشترین مقدار|max(3, 7, 2)|7|
|min(a, b, ...)|کمترین مقدار|min(3, 7, 2)|2|
|pow(x, y)|توان|pow(2, 3)|8|

📌 2. توابع نمایی، لگاریتم و ریشه (Exponentiation, Logarithm, Root)

|   |   |   |   |
|---|---|---|---|
|تابع|کاربرد|مثال|خروجی|
|math.exp(x)|عدد e به توان x|math.exp(1)|2.718...|
|math.log(x)|لگاریتم طبیعی (پایه e)|math.log(10)|2.302...|
|math.log10(x)|لگاریتم پایه 10|math.log10(100)|2.0|
|math.sqrt(x)|جذر (ریشه دوم)|math.sqrt(16)|4.0|
|math.pow(x, y)|توان (شبیه **)|math.pow(2, 3)|8.0|

توابع مثلثاتی (Trigonometric Functions)

|   |   |   |   |
|---|---|---|---|
|تابع|کاربرد|مثال|خروجی|
|math.sin(x)|سینوس زاویه (رادیان)|math.sin(math.pi / 2)|1.0|
|math.cos(x)|کسینوس|math.cos(0)|1.0|
|math.tan(x)|تانژانت|math.tan(math.pi / 4)|1.0|
|math.asin(x)|معکوس سینوس|math.asin(1)|π/2|
|math.acos(x)|معکوس کسینوس|math.acos(1)|0.0|
|math.atan(x)|معکوس تانژانت|math.atan(1)|π/4|
|math.degrees(x)|تبدیل رادیان به درجه|math.degrees(math.pi)|180.0|
|math.radians(x)|تبدیل درجه به رادیان|math.radians(90)|π/2|

توابع تصادفی و آماری (Random and Statistical Functions)

|   |   |   |   |
|---|---|---|---|
|تابع|کاربرد|مثال|خروجی|
|random.random()|عدد تصادفی بین ۰ و ۱|random.random()|مثلا 0.734|
|random.randint(a, b)|عدد صحیح تصادفی بین a و b|random.randint(1, 10)|مثلا 4|
|random.uniform(a, b)|عدد اعشاری تصادفی بین a و b|random.uniform(1, 5)|مثلا 3.14|
|random.choice(seq)|انتخاب تصادفی از لیست|random.choice(['a','b'])|'a' یا 'b'|
|random.shuffle(seq)|درهم‌ریزی لیست<br><br>سورتینگ لیست به‌صورت تصادفی|random.shuffle(my_list)||
|random.sample(seq, n)|انتخاب n عنصر تصادفی بدون تکرار|random.sample([1,2,3,4], 2)|مثلا [2, 4]|

توابع خاص و ثوابت (Special Functions & Constants)

|   |   |   |
|---|---|---|
|مورد|توضیح|مقدار|
|math.pi|عدد π|3.1415926535...|
|math.e|عدد e|2.7182818284...|
|math.inf|بی‌نهایت|math.inf|
|math.nan|مقدار "عدد نیست"|math.nan|
|math.factorial(n)|فاکتوریل|math.factorial(5) → 120|
|math.isfinite(x)|بررسی محدود بودن عدد|math.isfinite(100) → True|

# String Functions

در مواردیکه از str.~ استفاده شده است باید کتابخانه‌اش import گردد: import string

تغییر حروف بزرگ و کوچک Case Conversion

|   |   |   |   |
|---|---|---|---|
|تابع|کاربرد|مثال|خروجی|
|str.lower()|تبدیل تمام حروف به کوچک|'HELLO'.lower()|'hello'|
|str.upper()|تبدیل تمام حروف به بزرگ|'hi'.upper()|'HI'|
|str.title()|حرف اول هر کلمه بزرگ|'python code'.title()|'Python Code'|
|str.capitalize()|فقط حرف اول رشته بزرگ|'hello world'.capitalize()|'Hello world'|
|str.swapcase()|جابجایی بزرگ↔کوچک|'PyThOn'.swapcase()|'pYtHoN'|

تغییر و دستکاری رشته‌ها String Modification

🔹 درج و جایگذاری

|   |   |   |   |
|---|---|---|---|
|تابع|کاربرد|مثال|خروجی|
|'a' + 'b'|الحاق رشته|'Py' + 'thon'|'Python'|
|str.replace(old, new)|جایگزینی زیررشته|'hello'.replace('l', 'x')|'hexxo'|
|str.zfill(width)|پر کردن با صفر از چپ|'42'.zfill(5)|'00042'|

🔹 برعکس کردن رشته

|   |   |   |
|---|---|---|
|روش|مثال|خروجی|
|str[::-1]|'hello'[::-1]|'olleh'|

جستجو و بررسی رشته String Search & Validation

|   |   |   |   |
|---|---|---|---|
|تابع|کاربرد|مثال|خروجی|
|str.find(sub)|یافتن اولین مکان زیررشته|'hello'.find('l')|2|
|str.rfind(sub)|یافتن از انتها|'hello'.rfind('l')|3|
|str.startswith(prefix)|شروع با؟|'hello'.startswith('he')|True|
|str.endswith(suffix)|پایان با؟|'hello'.endswith('o')|True|
|str.isalpha()|فقط حروف؟|'abc'.isalpha()|True|
|str.isdigit()|فقط عدد؟|'123'.isdigit()|True|
|str.isalnum()|حروف یا عدد؟|'abc123'.isalnum()|True|
|str.isspace()|فقط فاصله؟|' '.isspace()|True|

مقایسه رشته‌ای String Comparison

|   |   |   |
|---|---|---|
|عملگر / تابع|مثال|نتیجه|
|==|'abc' == 'abc'|True|
|!=|'abc' != 'ABC'|True|
|<, >, <=, >=|'a' < 'b'|True|
|str.casefold()|مقایسه بدون حساسیت به حروف|'ß'.casefold() == 'ss'|

قالب‌بندی رشته‌ها String Formatting

|   |   |   |
|---|---|---|
|روش|مثال|خروجی|
|"{}".format(x)|"Hello {}".format('Ali')|'Hello Ali'|
|f"...{var}..."|f"Age: {age}"|'Age: 20'|
|%|"Name: %s" % 'Ali'|'Name: Ali'|

جدا کردن و چسباندن رشته‌ها String Splitting & Joining

|   |   |   |   |
|---|---|---|---|
|تابع|کاربرد|مثال|خروجی|
|str.split(sep)|جدا کردن با جداکننده|'a,b,c'.split(',')|['a', 'b', 'c']|
|str.rsplit(sep)|جدا کردن از راست|'a b c'.rsplit(' ', 1)|['a b', 'c']|
|'sep'.join(list)|الحاق لیست با جداکننده|'-'.join(['a','b'])|'a-b'|

حذف فاصله‌ها Trimming Whitespace

|   |   |   |   |
|---|---|---|---|
|تابع|کاربرد|مثال|خروجی|
|str.strip()|حذف فاصله ابتدا و انتها|' hi '.strip()|'hi'|
|str.lstrip()|فقط سمت چپ|' hi'.lstrip()|'hi'|
|str.rstrip()|فقط سمت راست|'hi '.rstrip()|'hi'|

توابع متفرقه Other Useful String Methods

|   |   |   |   |
|---|---|---|---|
|تابع|کاربرد|مثال|خروجی|
|str.count(sub)|تعداد وقوع یک زیررشته|'banana'.count('a')|3|
|str.center(width)|مرکز چین کردن رشته|'hi'.center(5)|' hi '|
|str.ljust(width)|چپ‌چین با فاصله|'hi'.ljust(5)|'hi '|
|str.rjust(width)|راست‌چین با فاصله|'hi'.rjust(5)|' hi'|
|str.partition(sep)|جدا کردن به ۳ قسمت<br><br>(قبل، جداکننده، بعد)|'key=value'.partition('=')|('key', '=', 'value')|