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