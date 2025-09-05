```table-of-contents

```

و Inertia دو روش اصلی برای ساخت فرم ارائه می‌دهد:
1. استفاده از کامپوننتِ \<Form>
2. استفاده از **useForm helper**
هر دو روش با اعتبارسنجی سمت سرور هماهنگ هستند و ارسال فرم بدون رفرش کامل صفحه را مدیریت می‌کنند.
---
# 1. component
این کامپوننت شبیه فرم HTML کلاسیک عمل می‌کند اما از Inertia برای جلوگیری از رفرش کامل استفاده می‌کند. ساده‌ترین روش برای شروع کار با فرم‌ها است.
```javascript
// ExampleForm.vue: Basic Inertia Form using <Form> component
<script setup>
import { Form } from '@inertiajs/vue3'
</script>
<template>
  <Form action="/users" method="post">
    <input type="text" name="name" />
    <input type="email" name="email" />
    <button type="submit">Create User</button>
  </Form>
</template>
```
نکات مهم:
- نیازی به v-model ندارید، فقط نام فیلدها را بدهید.
- کامپوننت به صورت خودکار داده‌ها را ارسال می‌کند.
### ۲. پشتیبانی از داده‌های تو در تو و فایل‌ها
```javascript
// ReportForm.vue: Form with nested data and file uploads
<Form action="/reports" method="post">
  <input type="text" name="name" />
  <textarea name="report[description]"></textarea>
  <input type="text" name="report[tags][]" />
  <input type="file" name="documents" multiple />
  <button type="submit">Create Report</button>
</Form>
```
- از **notation با نقطه یا آرایه** برای داده‌های تو در تو پشتیبانی می‌کند.
- می‌توان فایل‌ها را مستقیماً آپلود کرد.
### ۳. تغییر داده‌ها قبل از ارسال
```javascript
// PostForm.vue: Transforming form data before submission
<Form
  action="/posts"
  method="post"
  :transform="data => ({ ...data, user_id: 123 })"
>
  <input type="text" name="title" />
  <button type="submit">Create Post</button>
</Form>
```
- از prop `transform` برای تغییر یا افزودن فیلدها قبل از ارسال استفاده می‌شود.
- اگر نیازی به تغییر نداشتید، می‌توانید از hidden inputs استفاده کنید.
### کاربردهای عملی بدون کد
- ارسال فرم ثبت نام یا ورود بدون رفرش صفحه
- ساخت فرم‌های گزارش با چند فایل و آرایه داده‌ها
- افزودن خودکار فیلدهای جانبی مثل user_id قبل از ارسال فرم
- حفظ داده‌های فرم در صورت خطاهای سرور بدون از دست رفتن اطلاعات وارد شده

### [Wayfinder](obsidian://open?vault=obsidian&file=coding%20tutorials%2FLaravel%2FInertia%2FWayFunder) با component
وقتی از **Wayfinder** استفاده می‌کنید، می‌توانید به جای HTTP-Method، خروجی Wayfinder را مستقیماً به action بدهید. کامپوننتِ \<Form> به طور خودکار **HTTP method** و **URL** را از شیء Wayfinder استخراج می‌کند.
```javascript
// UserForm.vue: Using Wayfinder with Inertia <Form> component
<script setup>
import { Form } from '@inertiajs/vue3'
import { store } from 'App/Http/Controllers/UserController'
</script>
<template>
  <Form :action="store()"> //without WayFunder: <Form action="/users" method="post">
    <input type="text" name="name" />
    <input type="email" name="email" />
    <button type="submit">Create User</button>
  </Form>
</template>
```
نکات مهم:
- نیازی به مشخص کردن دستی مسیر URL یا HTTP-Method نیست، Wayfinder خودش این کار را انجام می‌دهد.
- این روش کدنویسی شما را کوتاه‌تر و خواناتر می‌کند.
- برای فرم‌های CRUD بسیار مناسب است، چون مسیر و روش درخواست را از سمت کنترلر لاراول می‌گیرد.
- کاهش خطاهای انسانی در مشخص کردن مسیرها و متدها هنگام ساخت فرم‌ها

### Checkbox inputs
در مورد **Checkbox inputs** در فرم‌های Inertia: وقتی یک چک‌باکس را استفاده می‌کنید، بهتر است مقدار صریح (`value`) به آن بدهید، مثل:
```html
<input type="checkbox" name="is_active" value="1">
```

دلیل این کار این است که اگر **مقدار value تعیین نشود**، زمانی که چک‌باکس انتخاب شود، مقدار `"on"` ارسال می‌شود.  بعضی از قواعد **اعتبارسنجی سمت سرور** این مقدار `"on"` را به عنوان یک مقدار بولی نمی‌شناسند. با تعیین مقدار مثلا `"1"` یا `"true"`، اعتبارسنجی راحت‌تر انجام می‌شود و داده ارسالی مطمئن است. 
فرض کنید یک فرم ثبت‌نام دارید و می‌خواهید کاربر تیک «عضویت در خبرنامه» را بزند یا نزند. اگر مقدار value را مشخص نکنید، ممکن است سرور نتواند تشخیص دهد که کاربر واقعاً این گزینه را فعال کرده یا خیر. با مقداردهی صریح مثل `value="1"`، وضعیت دقیق ارسال می‌شود.

## Slot props
کامپوننت `<Form>` **وضعیت واکنشی (reactive state)** و متدهای کمکی (helper methods) را از طریق **default slot** در اختیار شما می‌گذارد.
- این ویژگی‌ها شامل وضعیت پردازش فرم، خطاها و ابزارهای مفید برای مدیریت فرم هستند.
- با استفاده از **slot props**، می‌توانید وضعیت فرم و خطاها را بدون نیاز به متدهای جداگانه یا `v-model` مدیریت کنید.
مثال کامل از slot props با نمایش خطا و پیام موفقیت:
```vue
// FormExample.vue: Form with slot props for reactive state and helpers
<Form
  action="/users"
  method="post"
  #default="{
    errors,
    hasErrors,
    processing,
    progress,
    wasSuccessful,
    recentlySuccessful,
    setError,
    clearErrors,
    resetAndClearErrors,
    defaults,
    isDirty,
    reset,
    submit,
  }"
>
  <input type="text" name="name" />
  <div v-if="errors.name">
    {{ errors.name }}
  </div>
  <button type="submit" :disabled="processing">
    {{ processing ? 'Creating...' : 'Create User' }}
  </button>
  <div v-if="wasSuccessful">User created successfully!</div>
</Form>
```

مثال برای استفاده از **dotted notation** برای فرم‌های تو در تو:
```vue
// NestedFormExample.vue: Form with nested fields using dotted notation
<Form action="/users" method="post" #default="{ errors }">
  <input type="text" name="user.name" />
  <div v-if="errors['user.name']">{{ errors['user.name'] }}</div>
</Form>
```

جدول خلاصه slot props و کاربرد آنها:

| Prop / Method         | توضیح فارسی                                                           | نوع استفاده                             |
| --------------------- | --------------------------------------------------------------------- | --------------------------------------- |
| `errors`              | خطاهای اعتبارسنجی فرم، شامل ساختارهای تو در تو با **dotted notation** | نمایش خطاها                             |
| `hasErrors`           | بررسی اینکه آیا فرم خطا دارد یا خیر                                   | شرط‌بندی برای فعال/غیرفعال کردن دکمه‌ها |
| `processing`          | وضعیت در حال پردازش فرم (ارسال)                                       | تغییر متن دکمه‌ها یا نمایش loader       |
| `progress`            | وضعیت پیشرفت ارسال فایل یا فرم                                        | نمایش نوار پیشرفت                       |
| `wasSuccessful`       | بررسی موفقیت ارسال فرم                                                | نمایش پیام موفقیت                       |
| `recentlySuccessful`  | موفقیت اخیر فرم (برای مدت کوتاه)                                      | افکت‌های موقت                           |
| `setError`            | اضافه کردن یا اصلاح خطاها                                             | مدیریت خطاها به صورت دستی               |
| `clearErrors`         | پاک کردن خطاهای فرم                                                   | ریست کردن فرم                           |
| `resetAndClearErrors` | بازگردانی فرم به مقادیر اولیه و پاک کردن خطاها                        | reset فرم به حالت اول                   |
| `defaults`            | تنظیم مقادیر پیش‌فرض فرم مطابق مقادیر فعلی                            | تغییر default values برای reset بعدی    |
| `isDirty`             | بررسی اینکه آیا فرم تغییر کرده است یا خیر                             | نمایش پیام تغییرات ذخیره نشده           |
| `reset`               | بازگرداندن فرم به مقادیر پیش‌فرض                                      | بازگردانی فرم                           |
| `submit`              | ارسال فرم به سرور                                                     | جایگزین فرم HTML کلاسیک                 |
## Props and options
در کامپوننتِ `<Form>` ، علاوه بر `action` و `method`، تعدادی **props** دیگر نیز وجود دارند که بسیاری از آن‌ها مشابه گزینه‌های موجود در **Inertia visit options** هستند. این props و options به شما امکان می‌دهند نحوه ارسال فرم و رفتار بعد از آن را دقیقاً کنترل کنید.
مثال کامل:
```vue
// ProfileForm.vue: Form component with props and options
<Form
  action="/profile"
  method="put"
  error-bag="profile"
  query-string-array-format="indices"
  :headers="{ 'X-Custom-Header': 'value' }"
  :show-progress="false"
  :transform="data => ({ ...data, timestamp: Date.now() })"
  :invalidate-cache-tags="['users', 'dashboard']"
  disable-while-processing
  :options="{
    preserveScroll: true,
    preserveState: true,
    preserveUrl: true,
    replace: true,
    only: ['users', 'flash'],
    except: ['secret'],
    reset: ['page'],
  }"
>
  <input type="text" name="name" />
  <button type="submit">Update</button>
</Form>
```
توضیحات:
- بعضی props مانند `only`, `except`, `reset` زیر `options` گروه‌بندی شده‌اند تا با props سطح بالا که مربوط به **ارسال فرم** هستند، اشتباه گرفته نشوند.
- **قاعده کلی:** props سطح بالا برای **ارسال فرم** هستند، و `options` نحوه رفتار Inertia در بازدید بعدی را کنترل می‌کنند.
- و `disable-while-processing` هنگام پردازش فرم، ویژگی `inert` به تگ HTML فرم اضافه می‌شود تا از تعامل کاربر جلوگیری کند.

برای **استایل‌دهی فرم در حالت پردازش** می‌توان از کلاس‌های CSS یا Tailwind استفاده کرد، به عنوان مثال:
```vue
// Tailwind example for styling form while processing
<Form
    action="/profile"
    method="put"
    disableWhileProcessing
    className="inert:opacity-50 inert:pointer-events-none"
>
    <!-- Your form fields here -->
</Form>
```

جدول خلاصه props و options مهم:

|Prop / Option|توضیح فارسی|نوع استفاده|
|---|---|---|
|`action`|مسیر سروری که فرم به آن ارسال می‌شود|ضروری|
|`method`|روش HTTP فرم (get, post, put, patch, delete)|ضروری|
|`error-bag`|نام error bag برای مدیریت خطاهای اعتبارسنجی|نمایش خطاها|
|`query-string-array-format`|فرمت آرایه‌ها در query string|تنظیم URL|
|`headers`|اضافه کردن هدرهای دلخواه به درخواست|تنظیم درخواست|
|`show-progress`|نمایش یا عدم نمایش نوار پیشرفت|رابط کاربری|
|`transform`|تغییر داده‌های فرم قبل از ارسال|اصلاح داده‌ها|
|`invalidate-cache-tags`|پاکسازی cache بر اساس تگ‌ها|مدیریت کش|
|`disable-while-processing`|غیرفعال کردن فرم هنگام پردازش|جلوگیری از تعامل کاربر|
|`options.preserveScroll`|حفظ موقعیت اسکرول|رفتار scroll|
|`options.preserveState`|حفظ state کامپوننت|حفظ وضعیت فرم|
|`options.preserveUrl`|حفظ URL صفحه|مدیریت URL|
|`options.replace`|جایگزینی entry تاریخچه مرورگر|رفتار history|
|`options.only`|فقط بارگذاری props مشخص|partial reloads|
|`options.except`|حذف برخی props از بارگذاری|partial reloads|
|`options.reset`|ریست کردن فیلدهای مشخص|مدیریت فرم|
## Events Triggers
در کامپوننتِ `<Form>` ، می‌توانید **همه رویدادهای استاندارد visit** را برای ارسال فرم استفاده کنید. این رویدادها مشابه همان callbackهایی هستند که در `router.visit()` می‌توانستید استفاده کنید.
مثال:
```vue
// UserForm.vue: Form component emitting standard visit events
<Form
  action="/users"
  method="post"
  @before="handleBefore"       // Called before the visit starts
  @start="handleStart"         // Called when the visit starts
  @progress="handleProgress"   // Called with progress updates
  @success="handleSuccess"     // Called on successful response
  @error="handleError"         // Called on validation errors
  @finish="handleFinish"       // Called after visit finishes
  @cancel="handleCancel"       // Called if visit is cancelled
  @cancelToken="handleCancelToken" // Provides cancel token before visit
>
  <input type="text" name="name" />
  <button type="submit">Create User</button>
</Form>
```

| Prop / Event     | Type / Example        | توضیح فارسی                                                                           |
| ---------------- | --------------------- | ------------------------------------------------------------------------------------- |
| `@before`        | `(visit) => {}`       | قبل از شروع ارسال فرم فراخوانی می‌شود. می‌توانید با return `false` ارسال را لغو کنید. |
| `@start`         | `(visit) => {}`       | وقتی ارسال فرم آغاز می‌شود فراخوانی می‌شود.                                           |
| `@progress`      | `(progress) => {}`    | در حین ارسال فرم، پیشرفت بارگذاری فایل یا داده‌ها را نشان می‌دهد.                     |
| `@success`       | `(page) => {}`        | بعد از دریافت پاسخ موفق از سرور فراخوانی می‌شود.                                      |
| `@error`         | `(errors) => {}`      | در صورت خطاهای اعتبارسنجی، فراخوانی می‌شود.                                           |
| `@finish`        | `(visit) => {}`       | بعد از اتمام ارسال، چه موفقیت‌آمیز چه ناموفق، فراخوانی می‌شود.                        |
| `@cancel`        | `() => {}`            | وقتی ارسال فرم لغو می‌شود فراخوانی می‌شود.                                            |
| `@cancelToken`   | `(cancelToken) => {}` | قبل از ارسال، cancelToken ارائه می‌دهد تا بتوانید ارسال را دستی لغو کنید.             |


### Resetting the Form
و کامپوننتD `<Form>`  چند prop و گزینه برای **ریست کردن فرم بعد از ارسال** دارد:
- و `resetOnSuccess`: فرم را بعد از ارسال موفق ریست می‌کند.
- و `resetOnError`: فرم را بعد از دریافت خطا ریست می‌کند.
مثال‌ها:
```vue
// Reset entire form on success
<Form action="/users" method="post" resetOnSuccess>
  <input type="text" name="name" />
  <input type="email" name="email" />
  <button type="submit">Submit</button>
</Form>
// Reset specific fields on success
<Form action="/users" method="post" :resetOnSuccess="['name']">
  <input type="text" name="name" />
  <input type="email" name="email" />
  <button type="submit">Submit</button>
</Form>
// Reset entire form on error
<Form action="/users" method="post" resetOnError>
  <input type="text" name="name" />
  <input type="email" name="email" />
  <button type="submit">Submit</button>
</Form>
// Reset specific fields on error
<Form action="/users" method="post" :resetOnError="['name']">
  <input type="text" name="name" />
  <input type="email" name="email" />
  <button type="submit">Submit</button>
</Form>
```
این ویژگی‌ها مخصوص **مدیریت state فرم‌ها بعد از ارسال یا خطا** هستند و از دوباره‌سازی دستی فرم جلوگیری می‌کنند.
## Setting new default values
ویژگی `setDefaultsOnSuccess` به شما اجازه می‌دهد که **مقادیر فعلی فرم بعد از یک ارسال موفق را به عنوان مقادیر پیش‌فرض جدید تنظیم کنید**. این کار باعث می‌شود که بعد از reset کردن فرم، فیلدها به مقادیر تازه‌ای که کاربر وارد کرده بازگردند، نه مقادیر اولیه اولیه فرم.
```vue
// UserForm.vue: Form component setting new defaults on successful submission
<Form action="/users" method="post" setDefaultsOnSuccess>
  <input type="text" name="name" />
  <input type="email" name="email" />
  <button type="submit">Submit</button>
</Form>
```
## Dotted key notation
ویژگی **dotted key notation** در `<Form>` به شما اجازه می‌دهد تا با استفاده از نام‌های ورودی حاوی نقطه، ساختارهای تو در تو بسازید. این کار باعث می‌شود فرم داده‌ها را به صورت شیءهای تو در تو به سرور ارسال کند و مدیریت داده‌های پیچیده راحت‌تر شود.
مثال‌:
```vue
// UserForm.vue: Using dotted key notation for nested objects
<Form action="/users" method="post">
  <input type="text" name="user.name" />          // creates user.name
  <input type="text" name="user.skills[]" />      // creates user.skills as array
  <input type="text" name="address.street" />     // creates address.street
  <button type="submit">Submit</button>
</Form>
```
ساختار داده‌ای تولید شده:
```json
{
  "user": {
    "name": "John Doe",
    "skills": ["JavaScript"]
  },
  "address": {
    "street": "123 Main St"
  }
}
```

اگر نیاز داشته باشید که نقطه‌ها **واقعی باشند** و به عنوان جداساز شیء تو در تو استفاده نشوند، می‌توانید از backslash استفاده کنید:
```vue
// ConfigForm.vue: Escaping dots to keep literal keys
<Form action="/config" method="post">
  <input type="text" name="app\.name" />
  <input type="text" name="settings.theme\.mode" />
  <button type="submit">Save</button>
</Form>
```
ساختار داده‌ای تولید شده با escape:
```json
{
  "app.name": "My Application",
  "settings": {
    "theme.mode": "dark"
  }
}
```
## Programmatic access
ویژگی **Programmatic access** به شما اجازه می‌دهد تا به متدهای فرم به صورت برنامه‌ای (programmatically) دسترسی پیدا کنید و فرم را از خارج از خودش کنترل کنید. این روش جایگزین **slot props** است و مخصوصاً زمانی کاربرد دارد که می‌خواهید ارسال فرم یا ریست کردن آن را بدون تعامل مستقیم با فرم انجام دهید.
### مثال‌های کاربردی:
1. **ارسال فرم از یک دکمه جدا**  
    فرض کنید فرم ثبت‌نام کاربر در پایین صفحه است، اما می‌خواهید یک دکمه شناور در هدر صفحه داشته باشید که با کلیک روی آن فرم را ارسال کند. با دسترسی برنامه‌ای (programmatic access) می‌توانید این کار را بدون دخالت کاربر انجام دهید.
2. **ریست خودکار فرم پس از یک عمل دیگر**  
    مثلاً وقتی کاربر یک فیلتر را انتخاب می‌کند، فرم جستجو به صورت خودکار ریست شود تا داده‌ها دوباره بارگذاری شوند. این کار بدون نیاز به کلیک روی دکمه ریست انجام می‌شود.
3. **ارسال فرم پس از تایید کاربر**  
    اگر می‌خواهید قبل از ارسال فرم، یک پیغام تایید نشان دهید (مثل “آیا مطمئن هستید می‌خواهید ارسال کنید؟”)، می‌توانید از یک دکمه جدا استفاده کنید که ابتدا تاییدیه را نمایش دهد و سپس فرم را برنامه‌ای ارسال کند.
4. **ارسال فرم به صورت همزمان با سایر عملیات**  
    مثلاً وقتی کاربر اطلاعاتی را در چند فرم مختلف وارد کرده و دکمه “ذخیره همه” را می‌زند، می‌توانید همه فرم‌ها را برنامه‌ای ارسال کنید بدون اینکه کاربر تک‌تک دکمه‌ها را فشار دهد.
این مثال‌ها نشان می‌دهند که Programmatic access بیشتر برای سناریوهایی کاربرد دارد که می‌خواهید تعامل فرم را از بیرون کنترل کنید یا با منطق پیچیده ترکیب کنید.

```vue
// UserFormRef.vue: Accessing form methods programmatically using refs
<script setup>
import { ref } from 'vue'
import { Form } from '@inertiajs/vue3'
const formRef = ref()  // reference to the form
const handleSubmit = () => {
  formRef.value.submit() // programmatically submit the form
}
</script>
<template>
  <Form ref="formRef" action="/users" method="post">
    <input type="text" name="name" />
    <button type="submit">Submit</button>
  </Form>
  <button @click="handleSubmit">Submit Programmatically</button>
</template>
```
توضیح:
- در **Vue و React**، refs دسترسی به تمام متدها و state واکنشی فرم (مانند `isDirty` و `errors`) را فراهم می‌کنند.
- در **Svelte**، refs تنها متدها را ارائه می‌دهند و برای دسترسی به state واکنشی باید از slot props استفاده کنید.
# Form helper: useForm()
و `useForm` یک helper است که به شما اجازه می‌دهد کنترل برنامه‌ای روی داده‌ها و رفتار ارسال فرم داشته باشید، بدون نیاز به استفاده از کامپوننتِ `<Form>` .
### تعریف فرم
```javascript
// ExampleForm.vue: Define a reactive form using useForm
<script setup>
import { useForm } from '@inertiajs/vue3'
const form = useForm({
  email: null,
  password: null,
  remember: false,
})
</script>
```
### ارسال فرم
```html
<!-- ExampleForm.vue: Submit the form via a standard HTML form -->
<template>
  <form @submit.prevent="form.post('/login')">
    <input type="text" v-model="form.email">
    <div v-if="form.errors.email">{{ form.errors.email }}</div>
    <input type="password" v-model="form.password">
    <div v-if="form.errors.password">{{ form.errors.password }}</div>
    <input type="checkbox" v-model="form.remember"> Remember Me
    <button type="submit" :disabled="form.processing">Login</button>
  </form>
</template>
```
### متدهای ارسال
|Method|توضیح فارسی|
|---|---|
|`form.submit(method, url, options)`|ارسال فرم با هر متد دلخواه (`get`, `post`, `put`, `patch`, `delete`) و گزینه‌های سفارشی|
|`form.get(url, options)`|ارسال GET|
|`form.post(url, options)`|ارسال POST|
|`form.put(url, options)`|ارسال PUT|
|`form.patch(url, options)`|ارسال PATCH|
|`form.delete(url, options)`|ارسال DELETE|
### گزینه‌ها و ویژگی‌ها
|ویژگی|کاربرد|
|---|---|
|`preserveState`|حفظ وضعیت فرم در بازدیدهای بعدی|
|`preserveScroll`|حفظ موقعیت اسکرول|
|`onSuccess`|کال‌بک اجرا شده پس از موفقیت ارسال فرم، مثال: `form.reset('password')` برای ریست کردن فیلد پسورد|
|`transform`|تغییر داده‌ها قبل از ارسال به سرور، مثال: تبدیل `remember` به `'on'` یا `''`|
### ویژگی‌های UI
|ویژگی|کاربرد|
|---|---|
|`processing`|نشان می‌دهد که فرم در حال ارسال است و می‌توان دکمه submit را غیرفعال کرد|
|`progress`|برای نمایش درصد آپلود فایل‌ها|
### مثال transform
```javascript
form
  .transform((data) => ({
    ...data,
    remember: data.remember ? 'on' : '',
  }))
  .post('/login')
```
### مثال progress
```html
<progress v-if="form.progress" :value="form.progress.percentage" max="100">
  {{ form.progress.percentage }}%
</progress>
```
این helper برای سناریوهایی مناسب است که نیاز دارید فرم را از بیرون کامپوننت کنترل کنید، داده‌ها را دستکاری کنید، یا ارسال فرم را با منطق پیچیده مدیریت کنید.

هلپر `useForm` می‌تواند در بسیاری از مواقع جایگزین کامپوننتِ `<Form>` شود، مخصوصاً زمانی که می‌خواهید کنترل برنامه‌ای بیشتری روی فرم داشته باشید. 

| Slot props                 | و slot props در `<Form>` برای دسترسی به state و متدهای فرم بود؛ `useForm` همان داده‌ها و متدها را از طریق فرم reactive ارائه می‌دهد.         |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| Form component             | به جای استفاده از `<Form>` برای فرم‌های ساده، می‌توان از `useForm` استفاده کرد و فرم را به صورت برنامه‌ای کنترل کرد.                         |
| Props and options          | و props و گزینه‌های `<Form>` مثل action, method, headers و event callbacks در `useForm` با متدهای ارسال و گزینه‌های visit قابل مدیریت هستند. |
| Events                     | و event callbacks مانند onSuccess, onError, onFinish و… در `useForm` هنگام استفاده از متدهای ارسال (post, get, etc.) قابل استفاده هستند.     |
| Resetting the form         | ریست کردن فرم پس از submit با متد `form.reset()` یا `form.reset('fieldName')` مشابه `resetOnSuccess` و `resetOnError` است.                   |
| Setting new default values | با متد `form.defaults()` می‌توان مقادیر جدید پیش‌فرض را تعیین کرد، مشابه `setDefaultsOnSuccess` در `<Form>`.                                 |
| Dotted key notation        | و `useForm` به طور کامل از dotted key notation پشتیبانی می‌کند، مشابه `<Form>` برای ساخت nested objects.                                     |
| Programmatic access        | یکی از اصلی‌ترین مزایای `useForm` این است که از ابتدا به صورت برنامه‌ای طراحی شده است و نیاز به refs ندارد.                                  |
به طور خلاصه، هر جایی که `<Form>` نیاز به کنترل دقیق‌تر یا برنامه‌ای داشته باشد، `useForm` می‌تواند جایگزین آن شود. تنها جایی که `<Form>` هنوز ممکن است ترجیح داده شود، فرم‌های بسیار ساده با HTML سنتی هستند که نیازی به کنترل برنامه‌ای ندارند.
با سلام
### Form errors
در Inertia، خطاهای فرم از طریق پراپ `errors` قابل دسترسی هستند. وقتی از Laravel استفاده می‌کنید، اگر درخواست شما `ValidationException` پرتاب کند (مثلاً هنگام استفاده از <span dir=ltr>$request()->validate</span>)، این خطاها به‌طور خودکار در فرم پر می‌شوند.
**مثال نمایش خطا برای یک فیلد:**
```vue
<div v-if="form.errors.email">{{ form.errors.email }}</div> <!-- نمایش خطای ایمیل -->
```
### بررسی وجود خطا
با استفاده از property `hasErrors` می‌توانید بررسی کنید که آیا فرم خطا دارد یا خیر.
### پاک کردن خطاها
برای پاک کردن خطاها از متد `clearErrors()` استفاده کنید:
```js
// پاک کردن همه خطاها
form.clearErrors()
// پاک کردن خطاها برای فیلدهای خاص
form.clearErrors('field', 'anotherfield')
```
### تنظیم دستی خطاها
اگر از اعتبارسنجی سمت کلاینت استفاده می‌کنید، می‌توانید خطاها را با متد `setError()` تنظیم کنید:
```js
// تنظیم خطای یک فیلد
form.setError('field', 'Your error message.')
// تنظیم خطاهای چندگانه
form.setError({
  foo: 'Your error message for the foo field.',
  bar: 'Some other error for the bar field.'
})
```
توجه داشته باشید که در این حالت props صفحه تغییری نمی‌کند.
### موفقیت در ارسال فرم
- وقتی فرم با موفقیت ارسال شد، property `wasSuccessful` برابر با `true` می‌شود.
- و property `recentlySuccessful` برای دو ثانیه بعد از موفقیت نیز `true` می‌ماند و می‌توان از آن برای نمایش پیام‌های موفقیت موقت استفاده کرد.
با سلام
### Resetting the Form
برای بازگرداندن مقادیر فرم به مقادیر پیش‌فرض، از متد `reset()` استفاده می‌کنیم.
```js
// بازنشانی کل فرم
form.reset()
// بازنشانی فیلدهای خاص
form.reset('field', 'anotherfield')
```

اگر بخواهید همزمان مقادیر فرم را بازنشانی کرده و خطاهای اعتبارسنجی را پاک کنید، به جای فراخوانی جداگانه `reset()` و `clearErrors()`، می‌توانید از متد `resetAndClearErrors()` استفاده کنید.
```js
// بازنشانی فرم و پاک کردن همه خطاها
form.resetAndClearErrors()
// بازنشانی فیلدهای خاص و پاک کردن خطاهای آن‌ها
form.resetAndClearErrors('field', 'anotherfield')
```

در برنامه‌های وب، مخصوصاً زمانی که از فرم‌ها برای ارسال داده به سرور استفاده می‌کنید، بعد از یک ارسال موفق یا ناموفق ممکن است بخواهید مقادیر فرم را به حالت اولیه بازگردانید. این کار به دلایل زیر اهمیت دارد:
- **پاکسازی داده‌های قبلی:** اگر کاربر داده‌ای وارد کرده و فرم ارسال شده، مقادیر قبلی همچنان در فیلدها باقی بماند، می‌تواند باعث سردرگمی شود یا اطلاعات غلط ارسال شود.
- **آماده‌سازی فرم برای ورودی جدید:** پس از موفقیت در ثبت داده، بازنشانی فرم به کاربر اجازه می‌دهد که بدون حذف دستی مقادیر قبلی، داده جدید وارد کند.
- **مدیریت خطاهای اعتبارسنجی:** وقتی فرم خطاهای اعتبارسنجی داشته باشد، ترکیب بازنشانی فرم با پاک کردن خطاها باعث می‌شود فرم دوباره به حالت تمیز و آماده برای ورود داده تبدیل شود.
- `reset()` → بازنشانی مقادیر فرم به پیش‌فرض‌ها.
- `clearErrors()` → پاک کردن پیام‌های خطای اعتبارسنجی.
- `resetAndClearErrors()` → ترکیب دو عمل بالا در یک مرحله.
```js
// بازنشانی کل فرم
form.reset()
// بازنشانی فیلدهای خاص
form.reset('field', 'anotherfield')
// بازنشانی فرم و پاک کردن همه خطاها
form.resetAndClearErrors()
// بازنشانی فیلدهای خاص و پاک کردن خطاهای آن‌ها
form.resetAndClearErrors('field', 'anotherfield')
```
این روش به خصوص وقتی فرم پیچیده باشد یا شامل فیلدهای چندگانه و خطاهای اعتبارسنجی باشد، بسیار مفید است.
### Setting new default values
گاهی اوقات مقادیر پیش‌فرض فرم شما ممکن است قدیمی یا نادرست شوند، مثلاً بعد از یک عملیات موفق یا تغییر داده‌ها در سمت سرور. در چنین مواقعی متد `defaults()` به شما اجازه می‌دهد تا مقادیر پیش‌فرض فرم را به روز کنید. این کار باعث می‌شود زمانی که `reset()` را فراخوانی می‌کنید، فرم به مقادیر صحیح و به‌روز بازگردد.
**لغات تخصصی:**
- `defaults()` → به‌روزرسانی مقادیر پیش‌فرض فرم.
- `reset()` → بازگردانی فرم به مقادیر پیش‌فرض فعلی.

```js
// تنظیم مقادیر فعلی فرم به عنوان پیش‌فرض‌های جدید
form.defaults()
// به‌روزرسانی مقدار پیش‌فرض یک فیلد خاص
form.defaults('email', 'updated-default@example.com')
// به‌روزرسانی مقادیر پیش‌فرض چند فیلد
form.defaults({
  name: 'Updated Example',
  email: 'updated-default@example.com',
})
```
این روش برای فرم‌هایی که نیاز به حفظ وضعیت فعلی پس از موفقیت در ارسال یا تغییر داده‌ها دارند، بسیار مفید است و تجربه کاربری بهتری فراهم می‌کند.
### Form field change tracking
در هنگام کار با فرم‌ها، گاهی لازم است بدانیم کاربر آیا تغییراتی در فرم اعمال کرده است یا خیر. این موضوع برای جلوگیری از از دست رفتن داده‌های وارد شده یا هشدار دادن به کاربر قبل از خروج از صفحه بسیار مهم است. متد `isDirty` به شما اجازه می‌دهد وضعیت تغییرات فرم را بررسی کنید.
- `isDirty` → بررسی اینکه فرم نسبت به مقادیر پیش‌فرض تغییر کرده است یا خیر.
```vue
<!-- نمایش پیغام اگر فرم تغییر داشته باشد -->
<div v-if="form.isDirty">There are unsaved form changes.</div>
```
کاربرد عملی بدون کدنویسی: تصور کنید کاربری فرم ثبت سفارش را پر کرده و قصد دارد صفحه را ترک کند؛ با بررسی `isDirty` می‌توان قبل از خروج، هشدار "تغییرات ذخیره نشده وجود دارد" نمایش داد.
### Canceling Form submissions
گاهی لازم است فرم‌هایی که در حال ارسال هستند متوقف شوند، برای مثال اگر کاربر تصمیم گرفت ارسال را لغو کند یا کاربر به صفحه دیگری رفت. Inertia این امکان را با متد `cancel()` فراهم می‌کند.
- `cancel()` → لغو ارسال فعلی فرم قبل از کامل شدن آن.
pending submission : فرمی که هنوز ارسال نشده و در حال پردازش است.
```vue
<!-- لغو ارسال فرم -->
form.cancel()
```

اگر کاربری روی دکمه "لغو" کلیک کند یا تصمیم بگیرد فرم را دوباره ویرایش کند، می‌توان ارسال جاری را با `cancel()` متوقف کرد و از بروز خطا یا ارسال ناقص جلوگیری کرد.
### Form data and history state
گاهی لازم است داده‌ها و خطاهای یک فرم حتی بعد از تغییر صفحه یا بازگشت به صفحه قبلی حفظ شوند. Inertia این امکان را با ذخیره فرم در **history state** فراهم می‌کند. برای این کار، هنگام ساخت فرم با `useForm` می‌توانید یک **کلید یکتا** (unique form key) به عنوان آرگومان اول بدهید. این کلید باعث می‌شود Inertia بتواند داده‌ها و خطاهای فرم را در تاریخچه مرورگر ذخیره کند و هنگام بازگشت به صفحه دوباره بارگذاری کند.
- `history state` → حافظه مرورگر که وضعیت صفحات و فرم‌ها را ذخیره می‌کند.
unique form key : کلید یکتایی که فرم را در تاریخچه مرورگر شناسایی می‌کند
```js
// فرم ایجاد کاربر با کلید یکتا
const form = useForm('CreateUser', data)
// فرم ویرایش کاربر با شناسه کاربر در کلید
const form = useForm(`EditUser:${user.id}`, data)
```

مثال‌های کاربردی:
- کاربری در فرم ویرایش پروفایل، چند فیلد را پر کرده اما هنوز ذخیره نکرده؛ اگر مرورگر را عقب ببرد و دوباره به فرم برگردد، مقادیر قبلی حفظ شده و نیاز به پر کردن مجدد نیست.
- فرم ثبت سفارش در یک فروشگاه آنلاین: اگر کاربر بخواهد صفحه را تازه کند یا به صفحه قبل برگردد، محصولات انتخاب‌شده و خطاهای فرم همچنان نمایش داده می‌شوند.
- فرم ورود به حساب کاربری: پس از خطای اعتبارسنجی، ورودی‌های کاربر دوباره بارگذاری می‌شوند و لازم نیست دوباره وارد شوند.
## Wayfinder
و Wayfinder در Inertia به شما اجازه می‌دهد تا URL و متد HTTP را به صورت خودکار از روت‌‌های لاراول استخراج کنید و نیازی نباشد که دستی آن‌ها را مشخص کنید. این ویژگی مخصوصاً زمانی مفید است که بخواهید فرم‌ها یا درخواست‌های برنامه‌ریزی‌شده (programmatic requests) را با استفاده از helper فرم یا router انجام دهید.

سینتکس کلی استفاده از Wayfinder با فرم helper
```javascript
import { useForm } from '@inertiajs/vue3'
import { store } from 'App/Http/Controllers/UserController'
const form = useForm({
  name: 'John Doe',
  email: 'john.doe@example.com',
})
// ارسال فرم با استفاده از Wayfinder
form.submit(store())
```
**توضیحات:**
- و `store()` یک Wayfinder object است که مسیر و متد HTTP (مثلاً POST) را از کنترلر لاراول استخراج می‌کند.
- وقتی Wayfinder object را به `form.submit()` می‌دهید، فرم به طور خودکار URL و method صحیح را دریافت می‌کند.
- اگر بخواهید متد HTTP را به صورت دستی تغییر دهید، می‌توانید گزینه `method` را به `submit` بدهید، اما در این صورت این گزینه بر Wayfinder اولویت خواهد داشت.

مثال کاربردی:
- فرض کنید شما یک فرم ثبت کاربر دارید. به جای اینکه مسیر `POST /users` و داده‌ها را دستی مشخص کنید، فقط کافی است تابع `store()` کنترلر را بدهید. این باعث می‌شود فرم همیشه با مسیر درست و متد صحیح ارسال شود و اگر مسیر کنترلر تغییر کند، نیازی به تغییر کد فرانت‌اند نیست.
- همچنین وقتی فرم‌های مختلفی برای ایجاد یا ویرایش داده دارید، می‌توانید با Wayfinder فقط تابع مربوطه را بفرستید و فرم خودکار رفتار صحیح را انجام می‌دهد.
# Server-side responses
در استفاده از Inertia، معمولاً پاسخ‌های فرم را در سمت کلاینت مانند درخواست‌های معمولی XHR یا fetch بررسی نمی‌کنید. به جای آن، سرور شما (مثلاً از طریق route یا controller در لاراول) پس از پردازش فرم یک پاسخ redirect ارسال می‌کند، معمولاً به صفحه موفقیت یا همان صفحه‌ای که بعد از ثبت فرم باید نمایش داده شود.
#### مثال لاراول
```php
class UsersController extends Controller
{
    public function index()
    {
        return Inertia::render('Users/Index', [
          'users' => User::all(), // ارسال داده‌ها به صفحه
        ]);
    }
    public function store(Request $request)
    {
        // اعتبارسنجی داده‌ها و ایجاد رکورد جدید
        User::create($request->validate([
          'first_name' => ['required', 'max:50'],
          'last_name' => ['required', 'max:50'],
          'email' => ['required', 'max:50', 'email'],
        ]));
        // هدایت به صفحه لیست کاربران پس از موفقیت
        return to_route('users.index');
    }
}
```
**توضیحات:**
- این روش redirect-based با همه روش‌های ارسال فرم Inertia کار می‌کند:
- و   روش کامپوننتِ `<Form>` 
- و   روش هلپرِ `useForm`
- و   - ارسال دستی از طریق `router`
مزیت: تجربه فرم Inertia بسیار شبیه فرم‌های سنتی سمت سرور می‌شود، بدون اینکه لازم باشد پاسخ‌ها را در جاوااسکریپت پردازش کنید.

مثال: وقتی کاربر فرم ثبت کاربر جدید را پر می‌کند، پس از submit، فرم به صورت خودکار به صفحه لیست کاربران هدایت می‌شود. اگر validation سرور شکست بخورد، فرم دوباره همان صفحه را نشان می‌دهد و خطاها به صورت خودکار در فرم نمایش داده می‌شوند، بدون نیاز به مدیریت دستی خطا در کلاینت.
# Server-side validation
در Inertia، هم کامپوننتِ `<Form>`  و هم هلپر `useForm`  به‌طور خودکار خطاهای اعتبارسنجی سمت سرور را مدیریت می‌کنند. وقتی سرور شما خطاهای اعتبارسنجی را برمی‌گرداند، این خطاها به صورت خودکار در شیء `errors` قابل دسترسی هستند، بدون نیاز به تنظیمات اضافی در کلاینت.
**مزیت:** برخلاف درخواست‌های سنتی XHR یا fetch که ممکن است نیاز باشد کد وضعیت 422 را بررسی کنید، Inertia این خطاها را به عنوان بخشی از جریان redirect-based مدیریت می‌کند. این شبیه فرم‌های کلاسیک سمت سرور است، ولی بدون بارگذاری کامل صفحه.

مثال: کاربر یک فرم ثبت نام را پر می‌کند و اگر ایمیل یا نام خالی باشد، سرور خطای validation می‌دهد. فرم دوباره همان صفحه را نشان می‌دهد و خطاها به طور خودکار در کنار فیلدهای مربوطه نمایش داده می‌شوند. نیازی نیست در جاوااسکریپت وضعیت پاسخ یا خطاها را مدیریت کنید؛ همه چیز به صورت خودکار توسط Inertia انجام می‌شود.
# Manual form submissions
علاوه بر استفاده از `<Form>`  یا `useForm` ، می‌توانید فرم‌ها را به صورت دستی با استفاده از متدهای `router` Inertia ارسال کنید. در این روش، داده‌های فرم به صورت مستقیم به سرور ارسال می‌شوند و کنترل کامل روی درخواست دارید.
**مزیت:** این روش زمانی مفید است که بخواهید فرم‌های ساده یا فرم‌هایی که نیاز به مدیریت برنامه‌نویسی بیشتری دارند، بدون استفاده از کامپوننت‌های آماده، مدیریت کنید.
مثال کاربردی: کاربر یک فرم ثبت نام را پر می‌کند. با کلیک روی دکمه Submit، داده‌های فرم بدون بارگذاری صفحه به سرور ارسال می‌شوند. اگر سرور خطای validation داشته باشد، می‌توانید به صورت دستی یا با سایر مکانیزم‌ها خطاها را نمایش دهید.
این روش به شما انعطاف کامل می‌دهد تا قبل از ارسال، داده‌ها را پردازش یا اصلاح کنید. به عبارت دیگر، این روش همان رفتار فرم‌های کلاسیک را دارد ولی از Inertia برای ارسال AJAX بهره می‌برد.
# File uploads
هنگامی که فرم یا درخواست شما شامل فایل باشد، Inertia به طور خودکار داده‌ها را به یک **FormData** تبدیل می‌کند. این قابلیت در همه روش‌های ارسال فرم در Inertia قابل استفاده است:
- با **component**
- با **useForm helper**
- **ارسال دستی با router methods**
**مزیت:**
- نیازی نیست خودتان داده‌ها را به FormData تبدیل کنید.
- به راحتی می‌توانید فایل‌ها را به همراه سایر داده‌های فرم ارسال کنید.
- با استفاده از ویژگی‌های مربوط به **progress**، می‌توانید نوار پیشرفت آپلود را نشان دهید.
# XHR / fetch submissions
در اکثر موارد، **Inertia** برای ارسال فرم‌ها به صورت روان و بدون بارگذاری مجدد صفحه کافی است و نیازهای معمول را پوشش می‌دهد. با این حال، اگر نیاز به کنترل بیشتر روی ارسال فرم دارید، می‌توانید به صورت مستقیم از **XHR** یا **fetch** استفاده کنید و از کتابخانه یا متد دلخواه خود برای ارسال درخواست بهره ببرید.
**مثال‌های کاربردی:** 
- زمانی که بخواهید فرم را به یک API خارجی ارسال کنید که Inertia نمی‌تواند به‌صورت مستقیم با آن تعامل داشته باشد.
- زمانی که می‌خواهید پیش از ارسال، داده‌ها را پردازش یا رمزگذاری خاصی روی آنها انجام دهید.
- زمانی که می‌خواهید پاسخ سرور را به شکل سفارشی بررسی کنید و بر اساس آن رفتار متفاوتی داشته باشید (مثلاً نمایش اعلان‌ها یا هدایت به صفحه خاص).
**مزیت:**
- انعطاف‌پذیری کامل در مدیریت درخواست‌ها و پاسخ‌ها.
- کنترل دقیق روی هدرها، زمان‌بندی و مدیریت خطاها.
- همچنان می‌توانید از قابلیت‌های Inertia در بخش‌های دیگر برنامه استفاده کنید و تنها بخش فرم را به شکل دستی ارسال کنید.

این گزینه برای مواقعی مناسب است که نیازهای استاندارد Inertia پاسخگو نیست و باید مدیریت ارسال و دریافت داده‌ها به شکل سفارشی انجام شود.
```vue
<script setup>
import { ref } from 'vue'
const firstName = ref('')
const lastName = ref('')
const email = ref('')
const errors = ref({})
async function submitForm() {
  try {
    // ارسال داده‌ها با fetch
    const response = await fetch('/users', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'X-Custom-Header': 'MyHeaderValue'
      },
      body: JSON.stringify({
        first_name: firstName.value,
        last_name: lastName.value,
        email: email.value
      })
    })
    if (!response.ok) {
      // فرض کنیم سرور ValidationError داده
      const data = await response.json()
      errors.value = data.errors
      return
    }
    // موفقیت آمیز بود
    const data = await response.json()
    console.log('User created:', data)
  } catch (error) {
    console.error('Error submitting form:', error)
  }
}
</script>
<template>
  <form @submit.prevent="submitForm">
    <label>First name:</label>
    <input v-model="firstName" />
    <div v-if="errors.first_name">{{ errors.first_name }}</div>
    <label>Last name:</label>
    <input v-model="lastName" />
    <div v-if="errors.last_name">{{ errors.last_name }}</div>
    <label>Email:</label>
    <input v-model="email" />
    <div v-if="errors.email">{{ errors.email }}</div>
    <button type="submit">Submit</button>
  </form>
</template>
```
**مزایای این روش:**
- امکان استفاده از هدرهای سفارشی و پردازش داده‌ها قبل از ارسال.
- کنترل کامل روی مدیریت خطاها و پاسخ‌ها.
- مناسب برای ارسال فرم‌ها به API خارجی یا زمانی که می‌خواهید رفتار متفاوتی نسبت به جریان معمول Inertia داشته باشید.
# Summery
در ادامه یک قطعه کد کامل برای Vue (با Inertia.js و useForm) آورده‌ام که تقریباً تمام کاربردهای اصلی `useForm` را پوشش می‌دهد. سعی کردم درون کد تمام جزئیات را با کامنت توضیح بدهم:

```vue
<script setup>
// Import useForm helper from Inertia
import { useForm, router } from '@inertiajs/vue3'

// تعریف اولیه فرم با فیلدهای پیش‌فرض
const form = useForm({
  name: '',
  email: '',
  avatar: null,
})

// --- ارسال فرم با متد POST ---
// این روش ساده‌ترین حالت ارسال فرم است
function submitPost() {
  form.post('/users', {
    // گزینه‌های اضافی
    preserveScroll: true,    // اسکرول صفحه بعد از ارسال فرم حفظ شود
    preserveState: true,     // استیت قبلی صفحه حفظ شود
    onSuccess: () => {
      console.log('Form submitted successfully!')
    },
    onError: (errors) => {
      console.log('Validation failed:', errors)
    },
    onFinish: () => {
      console.log('Form request finished')
    },
  })
}

// --- ارسال فرم با متد PUT (مثال با spoofing) ---
function submitPut(userId) {
  form.post(`/users/${userId}`, {
    data: {
      _method: 'put', // Laravel روش PUT را شبیه‌سازی می‌کند
      ...form.data(),
    },
  })
}

// --- استفاده از transform ---
// تغییر داده‌ها قبل از ارسال (مثلاً تریم کردن name)
function submitWithTransform() {
  form.transform((data) => ({
    ...data,
    name: data.name.trim(),
  })).post('/users')
}

// --- مثال progress ---
// برای فایل آپلود
function submitFile() {
  form.post('/upload', {
    onProgress: (progress) => {
      console.log('Upload progress:', progress.percentage)
    },
  })
}

// --- بررسی خطاها ---
// form.errors شامل تمام خطاهای ولیدیشن است
function checkErrors() {
  if (form.hasErrors) {
    console.log('There are validation errors:', form.errors)
  }
}

// --- پاک کردن خطاهای خاص ---
function clearNameError() {
  form.clearErrors('name') // فقط خطای name پاک می‌شود
}

// --- تنظیم دستی خطاها ---
function setCustomError() {
  form.setError('email', 'This email is already taken.')
}

// --- موفقیت در ارسال فرم ---
// onSuccess داخل تنظیمات post استفاده می‌شود
// اما می‌توان بعداً هم بررسی کرد
function afterSuccess() {
  if (!form.hasErrors) {
    console.log('Everything went fine!')
  }
}

// --- Resetting the Form ---
function resetForm() {
  form.reset() // به مقادیر اولیه برمی‌گردد
}

// --- Setting new default values ---
function setNewDefaults() {
  form.defaults({ name: 'Guest', email: 'guest@example.com' })
}

// --- Form field change tracking ---
function trackChanges() {
  console.log('Was form changed?', form.isDirty)
}

// --- Canceling Form submissions ---
function cancelRequest() {
  form.cancel()
}

// --- Form data and history state ---
// با preserveState می‌توان فرم را در تاریخچه مرورگر نگه داشت
function submitWithHistory() {
  form.post('/users', { preserveState: true })
}

// --- Wayfinder ---
// به جای نوشتن دستی URL، می‌توان از Wayfinder برای مدیریت مسیرها استفاده کرد
// مثلاً: <Form :action="store()" method="post">
// در اینجا store() مسیر را تولید می‌کند
</script>

<template>
  <form @submit.prevent="submitPost">
    <!-- فیلدهای فرم -->
    <input type="text" v-model="form.name" placeholder="Name" />
    <div v-if="form.errors.name">{{ form.errors.name }}</div>

    <input type="email" v-model="form.email" placeholder="Email" />
    <div v-if="form.errors.email">{{ form.errors.email }}</div>

    <input type="file" @input="form.avatar = $event.target.files[0]" />
    <progress v-if="form.progress" :value="form.progress.percentage" max="100">
      {{ form.progress.percentage }}%
    </progress>

    <!-- وضعیت فرم -->
    <p v-if="form.processing">Submitting...</p>
    <p v-if="form.isDirty">Form has unsaved changes.</p>
    <p v-if="form.recentlySuccessful">Form submitted successfully!</p>

    <!-- دکمه ارسال -->
    <button type="submit">Submit</button>
    <button type="button" @click="resetForm">Reset</button>
  </form>
</template>
```


🔹 در این قطعه کد شما تقریباً همه ویژگی‌های مهم `useForm` را دارید:
- تعریف فرم و فیلدها
- ارسال با متدهای مختلف
- و transform برای تغییر داده‌ها
- و progress برای آپلود
- مدیریت خطاها (خواندن، پاک کردن، تنظیم دستی)
- و reset و defaults
- پیگیری تغییرات (isDirty)
- لغو درخواست (cancel)
- موفقیت و وضعیت UI
- استفاده از Wayfinder برای مسیرها

با سلام  
در ادامه یک قطعه کد کامل برای **کاربردهای مختلف `useForm` در Vue + Inertia** می‌آورم. تمام بخش‌ها با کامنت انگلیسی توضیح داده شده‌اند تا هر قابلیت مشخص باشد:

```vue
<script setup>
import { useForm, router } from "@inertiajs/vue3";

// -----------------------------
// 1) Define form with fields + default values
// -----------------------------
const form = useForm({
  name: "",
  email: "",
  password: "",
  avatar: null,
});

// -----------------------------
// 2) Submit form (basic POST request)
// -----------------------------
function submitBasic() {
  form.post("/users");
}

// -----------------------------
// 3) Submit with different HTTP methods
// -----------------------------
function submitPut() {
  form.put(`/users/${1}`); // update user
}
function submitPatch() {
  form.patch(`/users/${1}`); // partial update
}
function submitDelete() {
  form.delete(`/users/${1}`); // delete user
}

// -----------------------------
// 4) Options: preserveScroll, preserveState, replace, onSuccess, onError
// -----------------------------
function submitWithOptions() {
  form.post("/users", {
    preserveScroll: true,
    preserveState: true,
    replace: true,
    onSuccess: () => {
      console.log("Form submitted successfully!");
    },
    onError: (errors) => {
      console.log("Errors:", errors);
    },
    onFinish: () => {
      console.log("Always executed");
    },
  });
}

// -----------------------------
// 5) Transform example: modify data before sending
// -----------------------------
function submitWithTransform() {
  form.transform((data) => ({
    ...data,
    name: data.name.toUpperCase(),
  })).post("/users");
}

// -----------------------------
// 6) Progress bar (file upload)
// -----------------------------
function submitWithProgress() {
  form.post("/upload", {
    onProgress: (progress) => {
      console.log(progress.percentage + "% uploaded");
    },
  });
}

// -----------------------------
// 7) Form errors
// -----------------------------
function handleErrors() {
  // Check if error exists
  if (form.errors.email) {
    console.log("Email error:", form.errors.email);
  }

  // Manually set error
  form.setError("name", "Custom error on name field");

  // Clear single error
  form.clearErrors("name");

  // Clear all errors
  form.clearErrors();
}

// -----------------------------
// 8) Resetting the form
// -----------------------------
function resetForm() {
  form.reset(); // reset all fields
  form.reset("name", "email"); // reset specific fields
}

// -----------------------------
// 9) Setting new default values
// -----------------------------
function setNewDefaults() {
  form.defaults({
    name: "Default Name",
    email: "default@example.com",
  });
}

// -----------------------------
// 10) Form field change tracking
// -----------------------------
function trackChanges() {
  console.log("Is dirty?", form.isDirty); // true if any field changed
  console.log("Has changed name?", form.isDirty("name"));
}

// -----------------------------
// 11) Canceling form submissions
// -----------------------------
function cancelSubmission() {
  const req = form.post("/users");
  req.cancel(); // cancel request
}

// -----------------------------
// 12) Form data & history state
// -----------------------------
function checkFormState() {
  console.log(form.data()); // get current form data
  console.log(form.defaults()); // get default values
  console.log(form.recentlySuccessful); // true after successful submit
}

// -----------------------------
// 13) Wayfinder example
// Instead of <Form> component, directly use useForm
// -----------------------------
function wayfinderExample() {
  router.post("/users", form.data(), {
    onSuccess: () => {
      console.log("Submitted without <Form>");
    },
  });
}
</script>

<template>
  <div>
    <h2>useForm Complete Example</h2>

    <!-- Form UI -->
    <form @submit.prevent="submitBasic">
      <input type="text" v-model="form.name" placeholder="Name" />
      <input type="email" v-model="form.email" placeholder="Email" />
      <input type="password" v-model="form.password" placeholder="Password" />
      <input type="file" @input="form.avatar = $event.target.files[0]" />

      <!-- Progress UI -->
      <progress v-if="form.progress" :value="form.progress.percentage" max="100">
        {{ form.progress.percentage }}%
      </progress>

      <!-- Error UI -->
      <div v-if="form.errors.name" style="color:red">
        {{ form.errors.name }}
      </div>
      <div v-if="form.errors.email" style="color:red">
        {{ form.errors.email }}
      </div>

      <button type="submit" :disabled="form.processing">Submit</button>
    </form>
  </div>
</template>
```

مواردی که در این کد پوشش داده شدند:
- تعریف فرم و متدهای مختلف ارسال
- گزینه‌ها (مثل `onSuccess`, `onError`, ...)
- و transform و progress
- خطاها (بررسی، تنظیم دستی، پاک کردن)
- و reset و defaults
- ردیابی تغییرات (isDirty)
- لغو درخواست‌ها
- حالت‌های فرم و recentlySuccessful
- استفاده مستقیم از `router` به جای `<Form>` (Wayfinder)
## 📑 Cheatsheet — useForm (Inertia + Vue3)

|      Category       | method \| property                          | توضیح                                                        |
|:-------------------:| ------------------------------------------- | ------------------------------------------------------------ |
| **تعریف و داده‌ها** | `useForm(data)`                             | ساخت فرم با داده‌های اولیه                                   |
|                     | `form.data()`                               | دریافت داده‌های فعلی فرم                                     |
|                     | `form.defaults()`                           | گرفتن مقادیر پیش‌فرض                                         |
|                     | `form.reset(fields?)`                       | ریست کردن کل یا بخشی از فیلدها به مقدار پیش‌فرض              |
|                     | `form.setDefaults()`                        | تنظیم داده‌های فعلی به‌عنوان مقادیر پیش‌فرض جدید             |
|                     | `form.isDirty`                              | بررسی اینکه آیا داده‌ها نسبت به پیش‌فرض تغییر کرده‌اند یا نه |
|  **ارسال و متدها**  | `form.post(url, options)`                   | ارسال فرم با متد POST                                        |
|                     | `form.put(url, options)`                    | ارسال فرم با متد PUT                                         |
|                     | `form.patch(url, options)`                  | ارسال فرم با متد PATCH                                       |
|                     | `form.delete(url, options)`                 | ارسال فرم با متد DELETE                                      |
|                     | `form.get(url, options)`                    | ارسال فرم با متد GET                                         |
|                     | `form.transform(cb)`                        | تغییر داده‌ها قبل از ارسال (مثلاً حذف یا تغییر مقادیر)       |
|   **وضعیت و UI**    | `form.processing`                           | در حال ارسال بودن فرم (boolean)                              |
|                     | `form.progress`                             | وضعیت آپلود فایل (درصد، بایت ارسال‌شده، بایت کل)             |
|                     | `form.recentlySuccessful`                   | نمایش موفقیت‌آمیز بودن ارسال (مثلاً برای پیام موفقیت)        |
|      **خطاها**      | `form.errors`                               | لیست خطاهای اعتبارسنجی                                       |
|                     | `form.hasErrors`                            | بررسی وجود خطاها                                             |
|                     | `form.clearErrors(fields?)`                 | پاک کردن خطاها برای کل یا فیلد خاص                           |
|                     | `form.setError(field, message)`             | تنظیم دستی خطا برای یک فیلد خاص                              |
| **دیگر قابلیت‌ها**  | `form.cancel()`                             | لغو ارسال در حال انجام                                       |
|                     | `form.defaults()`                           | دریافت مقدار پیش‌فرض                                         |
|                     | `form.isDirty`                              | پیگیری تغییر مقادیر                                          |
|                     | `form.wasSuccessful`                        | بررسی موفقیت ارسال قبلی                                      |
|                     | `form.remember(key?)`                       | ذخیره داده‌های فرم در history state برای بازگشت بین صفحات    |
|    **Wayfinder**    | `<Form :action="route(...)" method="post">` | استفاده از کامپوننت `<Form>` برای هندل خودکار متدها و مسیرها |
- همیشه قبل از ارسال می‌توانید از `transform()` استفاده کنید تا داده‌ها اصلاح شوند.
- برای خطاها سه ابزار مهم دارید: `errors` (لیست خطاها)، `hasErrors` (چک سریع) و `clearErrors` (پاک کردن).
- `progress` فقط هنگام آپلود فایل کار می‌کند.
- اگر بخواهید حالت **method spoofing** (مثل PUT در فرم HTML) داشته باشید، می‌توانید `_method: 'put'` را به داده اضافه کنید.

|      category       | method \| property                          | توضیح                                     | مثال سناریویی                                                                                     |
|:-------------------:| ------------------------------------------- | ----------------------------------------- | ------------------------------------------------------------------------------------------------- |
| **تعریف و داده‌ها** | `useForm(data)`                             | ساخت فرم با داده‌های اولیه                | فرض کنید فرم ثبت‌نام دارید، فیلدهای نام و ایمیل را مقدار اولیه می‌دهید.                           |
|                     | `form.data()`                               | دریافت داده‌های فعلی فرم                  | مثلاً می‌خواهید قبل از ارسال ببینید کاربر الان چه چیزی وارد کرده است.                             |
|                     | `form.defaults()`                           | گرفتن مقادیر پیش‌فرض                      | کاربر بعداً هرچیزی تغییر دهد، این تابع هنوز مقادیر اولیه را نشان می‌دهد.                          |
|                     | `form.reset(fields?)`                       | ریست کردن کل یا بخشی از فیلدها            | اگر کاربر فرم را پر کرد ولی پشیمان شد، دکمه «پاک کردن» کلیک می‌کند و فرم برمی‌گردد به حالت اولیه. |
|                     | `form.setDefaults()`                        | تنظیم داده‌های فعلی به‌عنوان پیش‌فرض جدید | فرض کنید کاربر آدرس جدید ذخیره کرد، می‌خواهید همین آدرس پیش‌فرض بعدی باشد.                        |
|                     | `form.isDirty`                              | بررسی تغییر داده‌ها نسبت به پیش‌فرض       | مثلاً دکمه «خروج بدون ذخیره» فقط وقتی فعال شود که کاربر چیزی تغییر داده باشد.                     |
|  **ارسال و متدها**  | `form.post(url, options)`                   | ارسال فرم با متد POST                     | کاربر دکمه «ثبت سفارش» را می‌زند، اطلاعات جدید ایجاد می‌شود.                                      |
|                     | `form.put(url, options)`                    | ارسال فرم با متد PUT                      | کاربر اطلاعات پروفایل خود را ویرایش می‌کند و ذخیره می‌زند.                                        |
|                     | `form.patch(url, options)`                  | ارسال فرم با متد PATCH                    | مثلاً فقط ایمیل کاربر تغییر کرده، همان را آپدیت می‌کنید.                                          |
|                     | `form.delete(url, options)`                 | ارسال فرم با متد DELETE                   | کاربر می‌خواهد یک حساب یا محصول را حذف کند.                                                       |
|                     | `form.get(url, options)`                    | ارسال فرم با متد GET                      | برای گرفتن نتایج جستجو بدون رفرش کامل صفحه.                                                       |
|                     | `form.transform(cb)`                        | تغییر داده‌ها قبل از ارسال                | فرض کنید کاربر شماره تلفن وارد کرده با فاصله‌ها و خط تیره، قبل از ارسال آن‌ها را پاک می‌کنید.     |
|   **وضعیت و UI**    | `form.processing`                           | نشان دادن اینکه فرم در حال ارسال است      | مثلاً دکمه «ذخیره» را غیرفعال کنید تا دوباره کلیک نشود.                                           |
|                     | `form.progress`                             | وضعیت آپلود فایل                          | وقتی کاربر عکس آپلود می‌کند، نوار پیشرفت نمایش می‌دهید.                                           |
|                     | `form.recentlySuccessful`                   | وضعیت موفقیت اخیر                         | بعد از ثبت فرم، پیام سبز «با موفقیت ذخیره شد» نمایش داده می‌شود.                                  |
|      **خطاها**      | `form.errors`                               | لیست خطاهای اعتبارسنجی                    | اگر ایمیل خالی باشد، اینجا پیام خطا «ایمیل الزامی است» قرار می‌گیرد.                              |
|                     | `form.hasErrors`                            | بررسی وجود خطاها                          | برای اینکه بدانید لازم است خطاها را به کاربر نمایش دهید یا خیر.                                   |
|                     | `form.clearErrors(fields?)`                 | پاک کردن خطاها                            | وقتی کاربر دوباره در فیلد اشتباه تایپ می‌کند و خطا رفع می‌شود، پیام خطا پاک شود.                  |
|                     | `form.setError(field, message)`             | تنظیم دستی خطا                            | مثلاً اگر API خطای خاصی برگرداند که اعتبارسنجی نیست، می‌توانید آن را به یک فیلد وصل کنید.         |
| **دیگر قابلیت‌ها**  | `form.cancel()`                             | لغو ارسال در حال انجام                    | اگر کاربر وسط آپلود فایل «لغو» بزند، عملیات متوقف می‌شود.                                         |
|                     | `form.wasSuccessful`                        | بررسی موفقیت ارسال قبلی                   | برای اینکه بدانید آخرین بار فرم موفقیت‌آمیز بوده یا نه.                                           |
|                     | `form.remember(key?)`                       | ذخیره داده‌ها در history state            | اگر کاربر بین صفحات جابجا شد و برگشت، داده‌های فرم باقی بماند.                                    |
|    **Wayfinder**    | `<Form :action="route(...)" method="post">` | استفاده از کامپوننت فرم خودکار            | به جای نوشتن دستی متد و URL، فقط اکشن و متد را مشخص می‌کنید.                                      |
## \<Form> component

```vue
<script setup>
import { Form } from "@inertiajs/vue3";

// Custom callback handlers
function handleSuccess() {
  console.log("Form submitted successfully!");
}
function handleError(errors) {
  console.log("Validation errors:", errors);
}
function handleFinish() {
  console.log("This always runs after request finishes.");
}
function handleProgress(progress) {
  console.log("Upload progress:", progress.percentage + "%");
}
</script>

<template>
  <div>
    <h2>&lt;Form&gt; Component Example</h2>

    <!-- ----------------------------- -->
    <!-- 1) Basic usage with POST -->
    <!-- ----------------------------- -->
    <Form
      action="/users"
      method="post"
      :defaults="{ name: '', email: '', password: '' }"
    >
      <input type="text" name="name" placeholder="Name" />
      <input type="email" name="email" placeholder="Email" />
      <input type="password" name="password" placeholder="Password" />
      <button type="submit">Submit</button>
    </Form>

    <!-- ----------------------------- -->
    <!-- 2) With advanced options -->
    <!-- ----------------------------- -->
    <Form
      action="/users"
      method="post"
      :defaults="{ name: '', email: '' }"
      :preserve-scroll="true"
      :preserve-state="true"
      :replace="true"
      @success="handleSuccess"
      @error="handleError"
      @finish="handleFinish"
      @progress="handleProgress"
    >
      <input type="text" name="name" placeholder="Name" />
      <input type="email" name="email" placeholder="Email" />

      <!-- Progress bar -->
      <progress v-slot="{ progress }" v-if="progress" :value="progress.percentage" max="100">
        {{ progress.percentage }}%
      </progress>

      <!-- Error slots -->
      <div v-slot="{ errors }">
        <p v-if="errors.name" style="color:red">{{ errors.name }}</p>
        <p v-if="errors.email" style="color:red">{{ errors.email }}</p>
      </div>

      <button type="submit">Save</button>
    </Form>

    <!-- ----------------------------- -->
    <!-- 3) PUT / PATCH / DELETE methods -->
    <!-- ----------------------------- -->
    <Form :action="`/users/1`" method="put" :defaults="{ name: 'Old Name' }">
      <input type="text" name="name" />
      <button type="submit">Update (PUT)</button>
    </Form>

    <Form :action="`/users/1`" method="patch" :defaults="{ email: 'old@example.com' }">
      <input type="email" name="email" />
      <button type="submit">Update (PATCH)</button>
    </Form>

    <Form :action="`/users/1`" method="delete">
      <button type="submit" style="color:red">Delete</button>
    </Form>
  </div>
</template>
```

🔑 نکات مهم برای کامپوننتِ `<Form>` :
1. به جای `form.post()` و غیره، از پراپرتی `method="post|put|patch|delete"` استفاده می‌کنیم.
2. مقادیر اولیه فیلدها با `:defaults="{...}"` ست می‌شوند.
3. مدیریت خطا و موفقیت از طریق event ها (`@success`, `@error`, `@finish`, `@progress`) انجام می‌شود.
- و  `errors`, `progress`, `processing` و ... به‌صورت **slot props** در دسترس هستند.
1. ساده‌تر از `useForm` است وقتی نخواهی همه‌چیز را دستی هندل کنی.