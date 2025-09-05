با سلام،
### توضیح کلی درباره Wayfinder
**Wayfinder** یک ابزار کمکی در Inertia و Laravel است که مسیرهای سرور (server-side routes) و متدهای HTTP مربوط به آنها را به صورت یک **شیء قابل استفاده در جاوااسکریپت یا Vue/React/Svelte** فراهم می‌کند.
کاربرد اصلی Wayfinder این است که شما **نیازی به نوشتن URL و HTTP method به صورت دستی ندارید**. به جای آن، می‌توانید مستقیماً از متدهای کنترلر لاراول استفاده کنید و Wayfinder خودش URL و متد مناسب (GET, POST, PUT, PATCH, DELETE) را استخراج می‌کند.
مزایای Wayfinder:
- جلوگیری از اشتباه در وارد کردن مسیرها و متدهای HTTP
- کدنویسی کوتاه‌تر و خواناتر
- هماهنگی کامل با کنترلرهای لاراول و روش‌های CRUD
- مناسب برای استفاده با component و router.visit
مثال کاربردی بدون کدنویسی:
- وقتی می‌خواهید فرم ایجاد یا ویرایش یک کاربر را ارسال کنید، فقط کافی است متد مربوطه در کنترلر را به Wayfinder بدهید، بدون اینکه URL یا متد POST/PUT را صریحاً مشخص کنید.
- هنگام انجام عملیات حذف یا بروزرسانی، می‌توانید به راحتی شیء Wayfinder را به router.post یا router.visit بدهید و متد صحیح به طور خودکار انتخاب می‌شود.
با سلام،
### سینتکس کلی Wayfinder در Inertia
Wayfinder در دو سناریو اصلی استفاده می‌شود: **برای فرم‌ها** و **برای بازدیدهای دستی (manual visits)**. در هر حالت، Wayfinder به عنوان یک شیء شامل URL و متد HTTP عمل می‌کند.
#### 1. استفاده در فرم‌ها
```javascript
// Form component
<Form :action="WAYFINDER_OBJECT" method="optional">
  <input name="field1" />
  <input name="field2" />
  <button type="submit">Submit</button>
</Form>
```
- `WAYFINDER_OBJECT`: نتیجه فراخوانی متد کنترلر لاراول توسط Wayfinder است، مانند `store()`, `update(id)`, `destroy(id)`
- اگر `method` مشخص نشود، Wayfinder خودش متد صحیح (POST/PUT/PATCH/DELETE) را تعیین می‌کند.
- در صورت مشخص کردن `method`، مقدار آن اولویت دارد و جایگزین متد Wayfinder می‌شود.
#### 2. استفاده در بازدیدهای دستی (manual visits)
```javascript
import { router } from '@inertiajs/vue3'
router.visit(WAYFINDER_OBJECT, {
  method: 'optional',
  data: {},               // داده‌های اضافی
  replace: false,         // جایگزینی تاریخچه مرورگر
  preserveState: false,   // حفظ state کامپوننت
  preserveScroll: false,  // حفظ موقعیت اسکرول
  headers: {},            // هدرهای سفارشی
  onSuccess: page => {},  // callback موفقیت
  onError: errors => {},  // callback خطا
  onFinish: visit => {},  // callback پایان
})
```
- همانند فرم‌ها، Wayfinder به طور خودکار URL و متد HTTP مناسب را مشخص می‌کند.
- اگر `method` در گزینه‌ها مشخص شود، آن اولویت دارد.
#### 3. نمونه متدهای Wayfinder در کنترلر لاراول
```php
use App\Http\Controllers\UserController;
// ایجاد شیء Wayfinder
store()      // برای ایجاد (POST)
update(1)    // برای بروزرسانی (PUT/PATCH)
destroy(1)   // برای حذف (DELETE)
show(1)      // برای مشاهده (GET)
```
این سینتکس امکان استفاده **بدون دانستن URL و متد HTTP صریحاً** را فراهم می‌کند و هم در فرم‌ها و هم در manual visits کاربرد دارد.
اگر بخواهی، می‌توانم یک **جدول جمع‌بندی همه حالت‌ها و اولویت‌های Wayfinder** هم آماده کنم که سریع قابل مرور باشد. می‌خواهی این کار را انجام دهم؟


در زیر یک جدول کامل برای **Wayfinder در Inertia** آماده کرده‌ام که تمام حالت‌ها، کاربردها و اولویت‌ها را نشان می‌دهد:

|حالت استفاده|مثال|متد HTTP|توضیح|اولویت نسبت به `method` مشخص‌شده|
|---|---|---|---|---|
|فرم‌ها (`Form` component)|`<Form :action="store()">`|به صورت خودکار از Wayfinder گرفته می‌شود (POST)|Wayfinder URL و متد مناسب را تعیین می‌کند|اگر `method="PUT"` یا غیره مشخص شود، این مقدار جایگزین Wayfinder می‌شود|
|فرم‌ها با `method` مشخص|`<Form :action="update(1)" method="PATCH">`|PATCH|متد مشخص‌شده در فرم جایگزین متد Wayfinder می‌شود|مشخص‌شده در فرم|
|بازدید دستی (`router.visit`)|`router.visit(store())`|به صورت خودکار از Wayfinder گرفته می‌شود (POST)|URL و متد HTTP از شیء Wayfinder استخراج می‌شوند|اگر `method: 'PUT'` مشخص شود، آن اولویت دارد|
|بازدید دستی با `method`|`router.visit(update(1), { method: 'patch' })`|PATCH|متد مشخص‌شده در گزینه‌ها جایگزین متد Wayfinder می‌شود|مشخص‌شده در گزینه‌ها|
|سایر متدهای CRUD در کنترلر|`store(), update(1), destroy(1), show(1)`|POST/PUT/PATCH/DELETE/GET|ایجاد شیء Wayfinder برای عملیات CRUD|–|

