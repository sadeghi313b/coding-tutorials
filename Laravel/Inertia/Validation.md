با سلام
### Validation – How it works
در Inertia مدیریت **خطاهای اعتبارسنجی سمت سرور** کمی متفاوت از فرم‌های سنتی مبتنی بر XHR است. در فرم‌های کلاسیک، وقتی سرور خطای 422 برمی‌گرداند، شما مجبورید این خطاها را گرفته و دستی state فرم را به‌روزرسانی کنید. اما در Inertia چنین 422 پاسخ‌هایی هرگز ایجاد نمی‌شوند. روند کار به شکل زیر است:
1. **ارسال فرم با Inertia**  
    وقتی فرم خود را با Inertia ارسال می‌کنید، اگر خطاهای اعتبارسنجی وجود داشته باشد، سرور به جای ارسال JSON با وضعیت 422، کاربر را به فرم بازمی‌گرداند و خطاها را در session flash می‌کند. در فریمورک‌هایی مانند Laravel، این کار به صورت خودکار انجام می‌شود.
2. **دسترسی خودکار به خطاها در سمت کلاینت**  
    هر زمان که خطاها در session وجود داشته باشند، Inertia آنها را به صورت خودکار با page props به کلاینت ارسال می‌کند. این props واکنش‌گرا هستند و بنابراین به محض اتمام ارسال فرم، خطاها در فرم نمایش داده می‌شوند.
3. **تشخیص خطاها و فراخوانی callback مناسب**  
    از آنجا که هیچ پاسخ 422 ایجاد نمی‌شود، Inertia باید راه دیگری برای تشخیص خطاها داشته باشد. Inertia بررسی می‌کند که آیا `page.props.errors` شامل خطا هست یا خیر. اگر خطا وجود داشته باشد، callback مربوط به `onError()` فراخوانی می‌شود و callback `onSuccess()` اجرا نمی‌شود.
**مثال کاربردی بدون کدنویسی:**
- کاربر فرم ثبت‌نام را ارسال می‌کند و فیلد ایمیل خالی است.
- سرور کاربر را به همان فرم بازمی‌گرداند و پیام خطای "ایمیل الزامی است" در session flash می‌کند.
- Inertia به صورت خودکار این خطا را به فرم ارسال می‌کند و پیام خطا در کنار فیلد ایمیل نمایش داده می‌شود.
- callback `onError()` فراخوانی شده و می‌توانید برای نمایش پیام‌های اضافی یا لاگ کردن آنها استفاده کنید.
این روش باعث می‌شود فرم‌های Inertia شبیه فرم‌های کلاسیک سروری رفتار کنند ولی بدون **full page reload**.
با سلام
### Sharing errors در Inertia
برای اینکه خطاهای اعتبارسنجی سمت سرور در سمت کلاینت در دسترس باشند، باید این خطاها از طرف سرور از طریق prop به نام `errors` به اشتراک گذاشته شوند.
- اگر از **Laravel Inertia Adapter** استفاده کنید، این کار به صورت **اتوماتیک** انجام می‌شود. یعنی وقتی در Laravel از `validate()` یا `Validator` استفاده کنید، خطاها در session ذخیره می‌شوند و سپس توسط Inertia به صورت خودکار به کلاینت ارسال می‌گردند. در کلاینت، این خطاها در مسیر `page.props.errors` در دسترس خواهند بود.
- اگر از **فریمورک‌های دیگر** (مثل Rails، Django یا Express) استفاده می‌کنید، ممکن است لازم باشد این خطاها را به صورت دستی در props مربوط به Inertia ارسال کنید. این کار معمولاً با ارسال داده‌ی خطاها از سرور به view Inertia انجام می‌شود.
🔹 به طور خلاصه:
- در Laravel: هیچ کار اضافه‌ای نیاز نیست.
- در سایر فریمورک‌ها: باید خطاها را خودتان به `errors` prop پاس بدهید.
با سلام
بله، اجازه بدهید همین توضیح **Sharing errors** را با مثال‌های کدنویسی کامل کنم:
---
## ✅ 1. مثال در Laravel (اتوماتیک)
وقتی از `validate()` استفاده می‌کنید، Laravel به‌صورت پیش‌فرض خطاها را در **session** قرار می‌دهد و Inertia آنها را در `props.errors` به کلاینت می‌فرستد.
```php
// routes/web.php
use Illuminate\Http\Request;
use Inertia\Inertia;
Route::post('/users', function (Request $request) {
    $validated = $request->validate([
        'name'  => 'required|string',
        'email' => 'required|email|unique:users',
    ]);
    // اگر اعتبارسنجی موفق بود، ادامه اجرا می‌شود
    // وگرنه Laravel به صورت خودکار back() می‌زند
    // و خطاها را در session قرار می‌دهد.
    // ...
});
```
در Vue/React component:
```vue
<script setup>
import { usePage } from '@inertiajs/vue3'
const page = usePage()
const errors = page.props.errors
</script>
<template>
  <form method="post" action="/users">
    <input type="text" name="name" />
    <span v-if="errors.name">{{ errors.name }}</span>
    <input type="email" name="email" />
    <span v-if="errors.email">{{ errors.email }}</span>
    <button type="submit">Save</button>
  </form>
</template>
```
---
## ✅ 2. مثال در Node.js (Express + inertia-node)
در Express باید خطاها را **خودتان** به Inertia پاس بدهید:
```js
// routes/users.js
const express = require('express');
const router = express.Router();
router.post('/users', (req, res) => {
  const { name, email } = req.body;
  let errors = {};
  if (!name) errors.name = "Name is required";
  if (!email) errors.email = "Email is required";
  if (Object.keys(errors).length > 0) {
    // به جای ارسال 422، باید صفحه را دوباره رندر کنید
    return res.inertia('Users/Create', {
      errors, // اینجا خطاها را دستی پاس می‌دهید
      values: { name, email }
    });
  }
  // اگر خطایی نبود، ادامه‌ی ذخیره‌سازی
  // ...
  res.redirect('/users');
});
module.exports = router;
```
در Vue component:
```vue
<script setup>
import { usePage } from '@inertiajs/vue3'
const page = usePage()
const errors = page.props.errors
</script>
<template>
  <form method="post" action="/users">
    <input type="text" name="name" :value="page.props.values?.name" />
    <span v-if="errors.name">{{ errors.name }}</span>
    <input type="email" name="email" :value="page.props.values?.email" />
    <span v-if="errors.email">{{ errors.email }}</span>
    <button type="submit">Save</button>
  </form>
</template>
```
---
🔹 نتیجه:
- در **Laravel**: هیچ کاری لازم نیست (اتوماتیک).
- در **Express/Django/Rails**: باید خطاها را در `errors` به صورت دستی پاس بدهید.
با سلام
### Displaying errors در Inertia + Vue
در Vue (با استفاده از آداپتر Inertia Vue 3)، خطاهای اعتبارسنجی به صورت اتوماتیک از سرور در **props** با نام `errors` دریافت می‌شوند. بنابراین شما کافی است در صفحه‌ی Vue آنها را به صورت شرطی نمایش دهید.
---
### مثال کامل (Vue 3 + Inertia)
```vue
<script setup>
import { reactive } from 'vue'
import { router } from '@inertiajs/vue3'
// تعریف props برای گرفتن خطاها از سمت سرور
defineProps({
  errors: Object
})
// فرم ری‌اکتیو
const form = reactive({
  first_name: null,
  last_name: null,
  email: null,
})
// تابع ارسال فرم
function submit() {
  router.post('/users', form)
}
</script>
<template>
  <form @submit.prevent="submit" class="space-y-4">
    <!-- First Name -->
    <div>
      <label for="first_name">First name:</label>
      <input id="first_name" v-model="form.first_name" class="border p-2 rounded w-full" />
      <div v-if="errors.first_name" class="text-red-600 text-sm mt-1">
        {{ errors.first_name }}
      </div>
    </div>
    <!-- Last Name -->
    <div>
      <label for="last_name">Last name:</label>
      <input id="last_name" v-model="form.last_name" class="border p-2 rounded w-full" />
      <div v-if="errors.last_name" class="text-red-600 text-sm mt-1">
        {{ errors.last_name }}
      </div>
    </div>
    <!-- Email -->
    <div>
      <label for="email">Email:</label>
      <input id="email" v-model="form.email" type="email" class="border p-2 rounded w-full" />
      <div v-if="errors.email" class="text-red-600 text-sm mt-1">
        {{ errors.email }}
      </div>
    </div>
    <!-- Submit -->
    <button type="submit" class="bg-blue-500 text-white px-4 py-2 rounded">
      Submit
    </button>
  </form>
</template>
```
---
### نکات کلیدی
1. در سمت Laravel اگر از `request()->validate()` استفاده کنید، خطاها به صورت خودکار در `errors` قرار می‌گیرند و به Vue ارسال می‌شوند.
2. شما هم می‌توانید مستقیماً از `errors` که در props تعریف کرده‌اید استفاده کنید، هم از `this.$page.props.errors` (اما در Vue 3 ترکیب `defineProps` ساده‌تر و بهتر است).
## Repopulating input
در Inertia وقتی فرم ارسال می‌شود و اعتبارسنجی سرور خطا دارد، برخلاف روش‌های سنتی نیازی به پر کردن دوباره ورودی‌ها نیست. علت این است که Inertia در درخواست‌های `post`، `put`، `patch` و `delete` به‌طور پیش‌فرض **state کامپوننت را حفظ می‌کند**. بنابراین داده‌هایی که کاربر وارد کرده بود همچنان در همان فیلدها باقی می‌مانند.
کاری که توسعه‌دهنده باید انجام دهد فقط این است که خطاهای اعتبارسنجی را از طریق پراپ `errors` نمایش دهد. یعنی بازگرداندن مقادیر ورودی به صورت خودکار انجام می‌شود و هیچ منطق اضافی لازم نیست.
با سلام
## Error Bags
در Inertia، زمانی که فقط یک فرم در صفحه دارید، معمولاً نیازی به **Error Bag** نیست. زیرا خطاها به صورت خودکار به همان فرم مربوط می‌شوند.
اما اگر چندین فرم در یک صفحه داشته باشید، ممکن است خطاها با هم تداخل پیدا کنند.
---
### مشکل بدون Error Bag
فرض کنید دو فرم دارید:
- فرم ایجاد شرکت (**Create Company**)
- فرم ایجاد کاربر (**Create User**)
هر دو فرم یک فیلد مشترک به نام `name` دارند. اگر یکی از فرم‌ها خطا دهد، مثلاً نام شرکت نامعتبر باشد، خطا در هر دو فرم نمایش داده می‌شود چون هر دو از `errors.name` استفاده می‌کنند.
---
### راه‌حل با Error Bag
برای جلوگیری از این مشکل، از **Error Bag** استفاده می‌کنید. در این روش خطاها در یک کلید مجزا قرار می‌گیرند.
مثال مفهومی (بدون کدنویسی):
- خطاهای مربوط به فرم ایجاد شرکت در `errors.createCompany` ذخیره می‌شوند.
- خطاهای مربوط به فرم ایجاد کاربر در `errors.createUser` ذخیره می‌شوند.
---
### مزایای استفاده از Error Bag
- خطاهای هر فرم فقط در همان فرم نمایش داده می‌شوند.
- فیلدهای مشترک مانند `name` بین فرم‌ها دچار تداخل نمی‌شوند.
- مدیریت خطاها در صفحات پیچیده و چندفرمی ساده‌تر و شفاف‌تر می‌شود.
---
با سلام
### مثال‌های واقعی از Error Bag بدون کدنویسی
1. **سیستم مدیریت شرکت‌ها و کاربران**
    - در یک صفحه، یک فرم برای ثبت شرکت جدید دارید (فیلدها: نام شرکت، شماره ثبت).
    - در همان صفحه، یک فرم برای ایجاد کاربر جدید دارید (فیلدها: نام، ایمیل).
    - چون هر دو فرم فیلد `name` دارند، اگر خطای اعتبارسنجی برای نام شرکت رخ دهد، نباید در فرم کاربر نمایش داده شود. با استفاده از **Error Bag** خطاهای هر فرم جدا می‌شوند.
---
2. **صفحه پروفایل با دو بخش ویرایش**
    - یک فرم برای تغییر اطلاعات شخصی (نام، آدرس).
    - یک فرم برای تغییر رمز عبور (رمز فعلی، رمز جدید).
    - اگر کاربر در رمز عبور اشتباه وارد کند، این خطا نباید به فیلدهای بخش اطلاعات شخصی اعمال شود. هر بخش خطاهای خودش را دارد.
---
3. **پنل فروشگاه آنلاین**
    - یک فرم برای افزودن محصول (نام، قیمت، توضیحات).
    - یک فرم برای افزودن دسته‌بندی جدید (نام دسته، توضیحات).
    - چون هر دو فرم `نام` دارند، خطای اعتبارسنجی نام محصول نباید در فرم دسته‌بندی نمایش داده شود. Error Bag کمک می‌کند هر کدام خطاهای مخصوص به خودشان داشته باشند.
---
4. **داشبورد مالی**
    - فرم اول: ایجاد تراکنش جدید (مبلغ، توضیح).
    - فرم دوم: ایجاد حساب بانکی جدید (نام بانک، شماره حساب).
    - خطاهای حساب بانکی مثل "شماره حساب نامعتبر است" نباید در فرم تراکنش دیده شوند.
---
به زبان ساده: **Error Bag مثل یک جعبه جداگانه برای هر فرم است** که خطاها را فقط در همان جعبه نگه می‌دارد و جلوی قاطی شدن خطاهای فرم‌های مختلف را می‌گیرد.