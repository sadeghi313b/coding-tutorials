با سلام،
## Manual visits
### توضیح
علاوه بر استفاده از لینک‌ها، می‌توانید **درخواست‌های Inertia را به‌صورت برنامه‌ای (programmatically)** با استفاده از جاوااسکریپت ایجاد کنید.
- برای این کار از متد **`router.visit()`** استفاده می‌شود.
- این روش به شما امکان می‌دهد **پارامترها، روش HTTP، وضعیت scroll، state، headers و callbacks** را به صورت دقیق کنترل کنید.
- برای استفاده راحت‌تر، Inertia متدهای shortcut مثل `router.get()`، `router.post()` و غیره ارائه می‌دهد که تمام گزینه‌های `router.visit()` را دارند.
- متد `router.reload()` برای **بارگذاری مجدد صفحه فعلی** با `preserveState` و `preserveScroll` مناسب است.
### سینتکس کلی
```javascript
// PageComponent.vue: Programmatically visiting a URL with Inertia
import { router } from '@inertiajs/vue3'
// Manual visit using router.visit
router.visit(url, {
  method: 'get',           // HTTP method (get, post, put, patch, delete)
  data: {},                // Data to send with the request
  replace: false,          // Replace current history state instead of pushing a new one
  preserveState: false,    // Preserve local component state
  preserveScroll: false,   // Preserve scroll position
  only: [],                // Fetch only specific props from server
  except: [],              // Exclude specific props from server response
  headers: {},             // Custom headers
  errorBag: null,          // Validation error bag
  forceFormData: false,    // Force data to be sent as FormData
  queryStringArrayFormat: 'brackets', // Array format in query string
  async: false,            // Make the request asynchronously
  showProgress: true,      // Show progress bar
  fresh: false,            // Force fresh visit ignoring cache
  reset: [],               // Reset specific local component data
  preserveUrl: false,      // Preserve the current URL instead of updating it
  prefetch: false,         // Prefetch the visit
  onCancelToken: cancelToken => {},   // Callback for cancel token
  onCancel: () => {},                   // Callback when visit is cancelled
  onBefore: visit => {},                // Callback before request is sent
  onStart: visit => {},                 // Callback when request starts
  onProgress: progress => {},           // Callback for progress events
  onSuccess: page => {},                // Callback on successful response
  onError: errors => {},                // Callback on validation errors
  onFinish: visit => {},                // Callback when request finishes
  onPrefetching: () => {},              // Callback when prefetch starts
  onPrefetched: () => {},               // Callback when prefetch completes
})
// Shortcut methods
router.get(url, data, options)     // Send GET request
router.post(url, data, options)    // Send POST request
router.put(url, data, options)     // Send PUT request
router.patch(url, data, options)   // Send PATCH request
router.delete(url, options)        // Send DELETE request
router.reload(options)             // Reload current page with preserveState & preserveScroll
```
### مثال‌های کاربردی بدون کدنویسی
1. **ارسال فرم به‌صورت AJAX**  
    بدون رفرش کامل صفحه، فرم را ارسال و پاسخ را دریافت می‌کنیم.
2. **بارگذاری داده‌های جدید در جدول**  
    وقتی کاربر فیلتر یا جستجو می‌کند، داده‌ها با `router.get()` بارگذاری می‌شوند.
3. **حفظ state و scroll هنگام بروزرسانی جزئی صفحه**  
    مثلا فرم پر شده یا موقعیت کاربر در صفحه حفظ شود.
4. **بارگذاری صفحه فعلی دوباره**  
    با `router.reload()` می‌توان داده‌های جدید را دریافت کرد بدون از دست رفتن وضعیت فعلی فرم یا scroll.
با سلام،
### Method
زمانی که از **manual visits** استفاده می‌کنید، می‌توانید با گزینه‌ی **`method`**، **HTTP method** درخواست را مشخص کنید. مقادیر مجاز عبارتند از: `get`، `post`، `put`، `patch` و `delete`. مقدار پیش‌فرض `get` است.
- اگر بخواهید **فایل‌ها را آپلود کنید** و نیاز به `put` یا `patch` باشد، لاراول این حالت را مستقیماً پشتیبانی نمی‌کند. در این صورت باید از **form method spoofing** استفاده کنید: یعنی درخواست را با `post` ارسال کرده و یک فیلد `_method` با مقدار `put` یا `patch` اضافه کنید.
### سینتکس کلی
```javascript
// PageComponent.vue: Setting the HTTP method for a manual Inertia visit
import { router } from '@inertiajs/vue3'
router.visit(url, { 
  method: 'post', // HTTP method: get, post, put, patch, delete
  data: {}        // Optional data to send with the request
})
```
### مثال‌های کاربردی بدون کدنویسی
1. **ارسال فرم ثبت‌نام**  
    ارسال داده‌های فرم با `POST`.
2. **بروزرسانی اطلاعات پروفایل**  
    استفاده از `PUT` یا `PATCH` با form method spoofing.
3. **حذف یک رکورد**  
    استفاده از `DELETE` برای حذف آیتم از سرور.
با سلام،
### Wayfinder
**Wayfinder** ویژگی‌ای است که از نسخه‌ی Inertia **>= 2.1.2** پشتیبانی می‌شود. این قابلیت اجازه می‌دهد که یک شیء Wayfinder را مستقیم به هر متد router بدهید و **Inertia** خود به طور خودکار **HTTP method** و **URL** را از شیء Wayfinder استخراج می‌کند.
- اگر همزمان هم شیء Wayfinder بدهید و هم گزینه‌ی **`method`** را مشخص کنید، مقدار **`method`** بر مقدار پیش‌فرض Wayfinder ارجح خواهد بود.
### سینتکس کلی
```javascript
// ExampleComponent.vue: Using Wayfinder objects with Inertia router
import { router } from '@inertiajs/vue3'
import { show, store, destroy, update } from 'App/Http/Controllers/UserController'
// Visit a page inferred from Wayfinder object
router.visit(show(1))      // HTTP method and URL inferred from 'show'
// Send a POST request
router.post(store())        // HTTP method inferred as POST
// Send a DELETE request
router.delete(destroy(1))   // HTTP method inferred as DELETE
// Override method explicitly
router.visit(update(1), { method: 'patch' }) // method 'patch' takes precedence
```
### مثال‌های کاربردی بدون کدنویسی
1. **مشاهده جزئیات کاربر**  
    با استفاده از متد `visit` و شیء Wayfinder، بدون نیاز به مشخص کردن URL و method دستی.
2. **ثبت یک کاربر جدید**  
    ارسال مستقیم به endpoint ذخیره با استفاده از `store()`.
3. **حذف یک رکورد**  
    استفاده از `destroy()` برای حذف آیتم، بدون مشخص کردن دستی URL و method.
4. **بروزرسانی اطلاعات کاربر**  
    استفاده از `update()` با تعیین صریح method به `PATCH`.
با سلام،
### Data
گزینه‌ی **`data`** به شما اجازه می‌دهد که داده‌ها را به همراه یک **manual Inertia visit** ارسال کنید. این داده‌ها می‌توانند به صورت یک **object** یا **FormData** باشند.
- روش‌های shortcut مثل `get()`, `post()`, `put()`, و `patch()` نیز داده‌ها را به عنوان آرگومان دوم می‌پذیرند تا ارسال داده ساده‌تر شود.
### سینتکس کلی
```javascript
// ExampleComponent.vue: Sending data with a manual Inertia visit
import { router } from '@inertiajs/vue3'
// Using router.visit() with method and data
router.visit('/users', { 
  method: 'post',   // HTTP method
  data: {           // Data to send
    name: 'John Doe',
    email: 'john.doe@example.com'
  }
})
// Using shortcut method router.post() with data
router.post('/users', {
  name: 'John Doe',
  email: 'john.doe@example.com'
})
```
### مثال‌های کاربردی بدون کدنویسی
1. **ارسال فرم ثبت نام کاربر جدید**  
    پر کردن نام و ایمیل و ارسال به سرور.
2. **جستجوی کاربران با فیلتر**  
    ارسال یک query object برای محدود کردن نتایج.
3. **بروزرسانی اطلاعات پروفایل**  
    ارسال داده‌های تغییر یافته به سرور با استفاده از POST یا PATCH.
با سلام،
### Custom headers
گزینه‌ی **`headers`** به شما اجازه می‌دهد که **HTTP headers** دلخواه را به همراه درخواست Inertia ارسال کنید. توجه داشته باشید که هدرهایی که Inertia به صورت داخلی برای مدیریت وضعیت خود استفاده می‌کند اولویت دارند و قابل بازنویسی نیستند.
### سینتکس کلی
```javascript
// ExampleComponent.vue: Sending custom headers with Inertia request
import { router } from '@inertiajs/vue3'
router.post('/users', data, { 
  headers: {           // Custom headers to include in request
    'Custom-Header': 'value'
  } 
})
```
### مثال‌های کاربردی بدون کدنویسی
1. ارسال یک **توکن API** به سرور برای احراز هویت.
2. مشخص کردن نوع محتوا یا نسخه API با هدر `Accept` یا `Content-Type`.
3. افزودن **هدرهای سفارشی** برای کنترل رفتار سمت سرور در پاسخ به درخواست.
با سلام،
### File uploads
وقتی که درخواست‌ها شامل فایل هستند، Inertia به صورت خودکار داده‌ها را به یک **FormData object** تبدیل می‌کند تا فایل‌ها به درستی ارسال شوند. اگر بخواهید همیشه از **FormData** استفاده کنید، می‌توانید گزینه‌ی **`forceFormData`** را فعال کنید.
### سینتکس کلی
```javascript
// ExampleComponent.vue: Sending files with Inertia request
import { router } from '@inertiajs/vue3'
router.post('/companies', data, { 
  forceFormData: true  // Force the request to use FormData
})
```
### مثال‌های کاربردی بدون کدنویسی
1. آپلود تصویر پروفایل کاربر.
2. ارسال فایل اکسل یا PDF برای ثبت اطلاعات در سرور.
3. بارگذاری چند فایل همزمان از طریق فرم.
با سلام،
### Browser history
Inertia به صورت پیش‌فرض هر بازدید (visit) را به **history مرورگر** اضافه می‌کند، به این معنی که کاربران می‌توانند با دکمه‌های back و forward مرورگر بین صفحات جابه‌جا شوند.
با فعال کردن گزینه‌ی **`replace`**، بازدید جاری جایگزین آخرین ورودی در تاریخچه مرورگر می‌شود، به جای اینکه ورودی جدید ایجاد شود. این برای مواقعی کاربرد دارد که نمی‌خواهید کاربران با فشار دادن دکمه back به حالت قبلی بازگردند، مثل فیلترها یا جستجوهای موقت.
### سینتکس کلی
```javascript
// ExampleComponent.vue: Replacing current history entry
import { router } from '@inertiajs/vue3'
router.get('/users', { search: 'John' }, { 
  replace: true  // Replace current history entry instead of adding a new one
})
```
### مثال‌های کاربردی بدون کدنویسی
1. جستجو در یک جدول با بروزرسانی نتایج بدون اضافه کردن ورودی جدید در تاریخچه.
2. تغییر وضعیت فیلترها در یک صفحه بدون ایجاد رکورد جدید در back/forward.
3. فرم‌های ویرایش که بعد از ذخیره تغییرات، کاربر به همان صفحه بازمی‌گردد بدون اینکه ورودی اضافی در history ایجاد شود.
با سلام،
### Client side visits
شما می‌توانید از متدهای **`router.push`** و **`router.replace`** برای انجام بازدیدهای سمت کلاینت استفاده کنید. این روش مفید است وقتی می‌خواهید **تاریخچه مرورگر** را به‌روزرسانی کنید بدون اینکه یک درخواست سرور ارسال شود.
### سینتکس کلی
```javascript
// ExampleComponent.vue: Client-side navigation
import { router } from '@inertiajs/vue3'
// Push a new page state
router.push({
  url: '/users',                 // URL to display in the browser
  component: 'Users',            // Component to render
  props: { search: 'John' },     // Props to pass to the component
  clearHistory: false,           // Optionally clear the history stack
  encryptHistory: false,         // Optionally encrypt history state
  preserveScroll: false,         // Preserve current scroll position
  preserveState: false,          // Preserve current component state
  errorBag: null,                // Error bag name for validation errors
  onSuccess: (page) => {},       // Callback on successful visit
  onError: (errors) => {},       // Callback on error
  onFinish: (visit) => {},       // Callback when visit finishes
})
// Replace current page state
router.replace({
  props: (currentProps) => ({ ...currentProps, search: 'John' })
})
```
### نکات مهم
- همه پارامترها اختیاری هستند. به صورت پیش‌فرض، همه پارامترها به جز **`errorBag`** با props صفحه فعلی ترکیب می‌شوند.
- اگر نیاز دارید به props صفحه فعلی دسترسی پیدا کنید، می‌توانید یک **تابع** به `props` بدهید که props فعلی را دریافت کرده و props جدید را برگرداند.
- گزینه **`errorBag`** برای مشخص کردن کیسه خطاها در هنگام اعتبارسنجی استفاده می‌شود.
- مطمئن شوید هر مسیر سمت کلاینت که push یا replace می‌کنید، روی سرور نیز تعریف شده باشد تا در صورت رفرش صفحه، سرور بتواند صفحه را رندر کند.
### مثال‌های کاربردی بدون کدنویسی
1. جابه‌جایی بین تب‌های یک داشبورد بدون ارسال درخواست جدید به سرور.
2. فیلتر کردن لیست کاربران و به‌روزرسانی جدول در سمت کلاینت.
3. جایگزینی محتوای یک فرم با داده‌های جدید بدون ایجاد ورودی جدید در تاریخچه مرورگر.
با سلام،
### State preservation
به صورت پیش‌فرض، هر بازدید به همان صفحه باعث ایجاد یک نمونه جدید از کامپوننت صفحه می‌شود. این کار باعث می‌شود **هر حالت محلی** مثل مقدار فرم‌ها، موقعیت اسکرول، و فوکوس از بین برود.
گاهی لازم است که **حالت صفحه حفظ شود**. برای مثال، هنگام ارسال فرم، اگر اعتبارسنجی سمت سرور شکست بخورد، نیاز دارید داده‌های فرم حفظ شوند.
به همین دلیل، متدهای **post، put، patch، delete و reload** به صورت پیش‌فرض گزینه `preserveState` را روی `true` تنظیم می‌کنند.
برای حفظ حالت کامپوننت هنگام استفاده از **get**، می‌توانید `preserveState: true` را استفاده کنید.
### سینتکس کلی
```javascript
// ExampleComponent.vue: Preserving component state
import { router } from '@inertiajs/vue3'
// Preserve state unconditionally
router.get('/users', { search: 'John' }, { preserveState: true })
// Preserve state only if validation errors exist
router.get('/users', { search: 'John' }, { preserveState: 'errors' })
// Preserve state conditionally based on a callback
router.post('/users', data, {
  preserveState: (page) => page.props.someProp === 'value',
})
```
### نکات کلیدی
- حالت محلی کامپوننت شامل فرم‌ها، اسکرول، و فوکوس است.
- استفاده از `"errors"` باعث می‌شود که حالت فقط در صورت وجود خطای اعتبارسنجی حفظ شود.
- گزینه callback اجازه می‌دهد که بر اساس شرایط دلخواه تصمیم بگیرید که آیا state حفظ شود یا خیر.
### مثال‌های کاربردی بدون کدنویسی
1. حفظ مقادیر فرم هنگام ارسال فرم و برخورد با خطای اعتبارسنجی.
2. نگه داشتن موقعیت اسکرول در جدول یا لیست طولانی هنگام فیلتر یا جستجو.
3. حفظ مقدار ورودی در فرم جستجو هنگام ناوبری بین صفحات مشابه.
با سلام،
### Scroll preservation
به صورت پیش‌فرض، هنگام ناوبری بین صفحات، اینرتیا مانند مرورگر رفتار می‌کند و **موقعیت اسکرول صفحه و بخش‌های قابل اسکرول را به بالای صفحه بازمی‌گرداند**.
برای جلوگیری از این رفتار، می‌توانید گزینه `preserveScroll: true` را استفاده کنید.
### سینتکس کلی
```javascript
// ExampleComponent.vue: Preserving scroll position
import { router } from '@inertiajs/vue3'
// Preserve scroll unconditionally
router.visit(url, { preserveScroll: true })
// Preserve scroll only if validation errors exist
router.visit(url, { preserveScroll: 'errors' })
// Preserve scroll conditionally based on a callback
router.post('/users', data, {
  preserveScroll: (page) => page.props.someProp === 'value',
})
```
### نکات کلیدی
- این گزینه برای صفحات طولانی، جدول‌ها یا لیست‌ها بسیار مفید است.
- استفاده از `"errors"` باعث می‌شود که موقعیت اسکرول فقط در صورت وجود خطای اعتبارسنجی حفظ شود.
- گزینه callback اجازه می‌دهد که بر اساس شرایط دلخواه تصمیم بگیرید که آیا اسکرول حفظ شود یا خیر.
### Partial reloads
گزینه `only` به شما اجازه می‌دهد که **فقط مجموعه‌ای از props ها را در بازدیدهای بعدی از سرور درخواست کنید**. این کار باعث می‌شود که برنامه شما کارآمدتر باشد و داده‌های غیر ضروری دوباره بارگذاری نشوند.
### سینتکس کلی
```javascript
// ExampleComponent.vue: Partial reloads
import { router } from '@inertiajs/vue3'
// Reload only specific props
router.get('/users', { search: 'John' }, { only: ['users'] })
```
### نکات کلیدی
- استفاده از `only` باعث کاهش مصرف پهنای باند و افزایش سرعت می‌شود.
- مناسب برای صفحات دارای داده‌های بزرگ یا لیست‌های طولانی که همه داده‌ها نیاز به بروزرسانی ندارند.
با سلام،
### Visit cancellation
در Inertia، شما می‌توانید **بازدید (visit) در حال اجرا را لغو کنید**. این کار با استفاده از **cancel token** امکان‌پذیر است. این token قبل از شروع بازدید از طریق `onCancelToken()` در دسترس قرار می‌گیرد.
### سینتکس کلی
```javascript
// ExampleComponent.vue: Canceling an Inertia visit
import { router } from '@inertiajs/vue3'
// Register cancel token before visit
router.post('/users', data, {
  onCancelToken: (cancelToken) => (this.cancelToken = cancelToken),
})
// Later, cancel the visit
this.cancelToken.cancel()
```
### نکات کلیدی
- **onCancelToken**: قبل از شروع بازدید، cancel token را در اختیار شما می‌گذارد.
- **cancel()**: با فراخوانی این متد، بازدید جاری لغو می‌شود.
- **onCancel و onFinish**: هنگام لغو بازدید، این callbackها اجرا خواهند شد.
### کاربردهای عملی بدون کدنویسی
- لغو یک جستجوی طولانی در صفحه قبل از دریافت نتایج کامل.
- جلوگیری از ارسال فرم‌ها به سرور هنگام تغییر صفحه یا اقدام کاربر قبل از اتمام درخواست.
- مدیریت چندین درخواست همزمان و جلوگیری از تداخل داده‌ها.
با سلام،
### Event callbacks
در Inertia، علاوه بر **رویدادهای سراسری (global events)**، می‌توانید از **callbackهای مخصوص هر بازدید (per-visit)** نیز استفاده کنید. این callbackها به شما امکان می‌دهند رفتار بازدید، مدیریت خطا و تغییر وضعیت صفحه را دقیق‌تر کنترل کنید.
### سینتکس کلی
```javascript
// ExampleComponent.vue: Per-visit event callbacks
import { router } from '@inertiajs/vue3'
router.post('/users', data, {
  onBefore: (visit) => {},      // قبل از شروع بازدید اجرا می‌شود. بازگرداندن false باعث لغو بازدید می‌شود.
  onStart: (visit) => {},       // درست بعد از شروع بازدید اجرا می‌شود.
  onProgress: (progress) => {}, // در طول ارسال داده‌ها برای دنبال کردن پیشرفت اجرا می‌شود.
  onSuccess: (page) => {},      // هنگام موفقیت بازدید اجرا می‌شود.
  onError: (errors) => {},      // هنگام دریافت خطاهای سرور اجرا می‌شود.
  onCancel: () => {},           // هنگام لغو بازدید اجرا می‌شود.
  onFinish: (visit) => {},      // پس از پایان بازدید، حتی در صورت خطا یا لغو، اجرا می‌شود.
  onPrefetching: () => {},      // قبل از پیش‌بارگذاری داده‌ها اجرا می‌شود.
  onPrefetched: () => {},       // بعد از پیش‌بارگذاری داده‌ها اجرا می‌شود.
})
```
### نکات مهم
- **onBefore**: می‌توانید false برگردانید تا بازدید لغو شود، مثلاً برای تایید کاربر.
- **onSuccess / onError**: می‌توانید یک Promise برگردانید تا اجرای **onFinish** تا اتمام Promise به تأخیر بیفتد.
- این callbackها به شما امکان می‌دهند:
    - نمایش پیام بارگذاری یا نوتیفیکیشن هنگام بازدید
    - مدیریت خطاهای سرور برای فرم‌ها
    - اجرای کارهای بعد از موفقیت بازدید، مانند به‌روزرسانی state محلی
### کاربردهای عملی بدون کدنویسی
- نمایش پیغام تایید قبل از حذف یک رکورد.
- نمایش progress bar هنگام ارسال فرم طولانی.
- اجرای چندین عملیات بعد از موفقیت بازدید، قبل از تغییر صفحه یا اطلاع‌رسانی کاربر.