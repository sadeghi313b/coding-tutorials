### تعریف کلی
در **Inertia.js**، هر صفحه (Page) در برنامه معمولاً یک **Controller/Route** مخصوص به خودش در لاراول دارد و در سمت فرانت‌اند نیز یک **کامپوننت جاوااسکریپت (Vue/React/Svelte)** متناظر با آن وجود دارد.
### مثال ساده
فرض کنید یک صفحه **لیست کاربران** دارید:
1. در سمت بک‌اند (لاراول)، یک کنترلر مثل `UserController@index` وجود دارد.
2. این کنترلر با `Inertia::render`، داده‌های کاربران را به یک کامپوننت Vue (مثلاً `Users/Index.vue`) می‌فرستد.
3. در سمت فرانت‌اند، `Users/Index.vue` داده‌ها را آماده دارد و بدون نیاز به درخواست اضافه‌ی Ajax، صفحه را کامل رندر می‌کند.
## Creating Pages
### تعریف
در Inertia، **صفحات (Pages)** چیزی جز **صفحات جاوااسکریپت** نیستند.  
اگر تجربه‌ی کار با Vue، React یا Svelte را داشته باشید، به‌راحتی می‌توانید صفحات Inertia را بسازید.
صفحات داده‌های موردنیاز خود را از **کنترلرها** (در لاراول) به صورت **props** دریافت می‌کنند.
### مثال در Vue
```vue
<script setup>
import Layout from './Layout'
import { Head } from '@inertiajs/vue3'
// تعریف props دریافتی از سرور
defineProps({ user: Object })
</script>
<template>
  <Layout>
    <!-- تعیین عنوان صفحه -->
    <Head title="Welcome" />
    <h1>Welcome</h1>
    <p>Hello {{ user.name }}, welcome to your first Inertia app!</p>
  </Layout>
</template>
```
🔹 در اینجا:
کامپوننت `Layout` یک چیدمان است که صفحه داخل آن قرار می‌گیرد.
تگ `Head` برای مدیریت متادیتا استفاده می‌شود.
پراپ `user` از سمت بک‌اند به این صفحه ارسال می‌شود.
---
### اتصال با بک‌اند (Laravel)
برای رندر کردن این صفحه در لاراول، کافیست از **Inertia::render** استفاده کنیم:
```php
use Inertia\Inertia;
class UserController extends Controller
{
    public function show(User $user)
    {
        return Inertia::render('User/Show', [
            'user' => $user
        ]);
    }
}
```
🔹 در اینجا:
- متد `show` یک مدل `User` دریافت می‌کند.
- داده‌ی کاربر (`$user`) به کامپوننت Vue (`User/Show.vue`) ارسال می‌شود.
- مسیر فایل صفحه در این مثال:  
    `resources/js/Pages/User/Show.vue`
---
### نکته مهم
اگر سعی کنید صفحه‌ای را رندر کنید که وجود ندارد:
- به طور پیش‌فرض یک صفحه‌ی خالی نمایش داده می‌شود.
- برای جلوگیری از این مشکل، می‌توانید در فایل تنظیمات `config/inertia.php` گزینه‌ی `ensure_pages_exist` را روی `true` قرار دهید.
- در این حالت، اگر صفحه پیدا نشود، لاراول خطای `Inertia\ComponentNotFoundException` را پرتاب خواهد کرد.
---
👉 خلاصه:  
صفحات در Inertia همان **کامپوننت‌های Vue/React/Svelte** هستند که داده‌ها را به صورت props از سرور دریافت می‌کنند. در بک‌اند لاراول با `Inertia::render` آن‌ها را فراخوانی می‌کنیم و در صورت عدم وجود صفحه، می‌توانیم خطای مشخصی دریافت کنیم.
## Creating layouts
### مثال در Vue
```vue
<script setup>
import { Link } from '@inertiajs/vue3'
</script>
<template>
  <main>
    <header>
      <Link href="/">Home</Link>
      <Link href="/about">About</Link>
      <Link href="/contact">Contact</Link>
    </header>
    <article>
      <slot />
    </article>
  </main>
</template>
```
---
### توضیح کد
- کتابخانه `@inertiajs/vue3` شامل لینک‌های ناوبری مخصوص Inertia است.
- لینک `Link` مشابه تگ `<a>` عمل می‌کند، اما بدون رفرش کامل صفحه بین مسیرها جابه‌جا می‌شود.
- بخش `<header>` شامل لینک‌های ناوبری به صفحات مختلف است.
- بخش `<slot />` محلی است که محتوای هر صفحه در آن قرار می‌گیرد. به این صورت، همه صفحات داخل یک قالب مشترک نمایش داده می‌شوند.
---
---
---
## Persistent layouts
### توضیح
وقتی یک صفحه درون یک Layout قرار بگیرد ولی این Layout در هر بار تغییر صفحه دوباره ساخته و نابود شود، وضعیت داخلی آن از بین می‌رود. با استفاده از Persistent Layouts می‌توان المان‌هایی مانند موزیک پلیر، سایدبار با وضعیت ثابت یا هر داده‌ای که باید بین صفحات باقی بماند را حفظ کرد و تجربه کاربری روان‌تری ایجاد نمود. این موضوع مشکلاتی مانند این موارد ایجاد می‌کند:
- پخش‌کننده موسیقی در یک وبسایت پادکست هنگام تغییر صفحه متوقف می‌شود.
- موقعیت اسکرول سایدبار یا باز/بسته بودن منو با تغییر صفحه ریست می‌شود.
برای رفع این مشکل در Inertia از **Persistent Layouts** استفاده می‌کنیم. در Vue 3.3 و بالاتر می‌توان به‌سادگی با استفاده از `defineOptions` این کار را انجام داد.
---
### مثال ساده با Vue 3.3+
```vue
// This is a page component that will be rendered inside a Layout
<script setup>
import Layout from './Layout'
defineOptions({ layout: Layout })
defineProps({ user: Object })
</script>
<template>
  <h1>Welcome</h1>
  <p>Hello {{ user.name }}, welcome to your first Inertia app!</p>
</template>
```
---
### توضیح کد
- تابع `defineOptions` برای مشخص کردن تنظیمات کامپوننت استفاده می‌شود.
- گزینه `layout` تعیین می‌کند که این صفحه داخل کدام Layout نمایش داده شود.
- از آنجایی که Layout به صورت پایدار باقی می‌ماند، وضعیت داخلی آن (مثل پخش موسیقی یا موقعیت اسکرول) حفظ خواهد شد.
---
### Nested Layouts با Vue 3.3+
می‌توان بیش از یک Layout استفاده کرد و آن‌ها را به صورت تو در تو تعریف نمود:
```vue
<script setup>
import SiteLayout from './SiteLayout'
import NestedLayout from './NestedLayout'
defineOptions({ layout: [SiteLayout, NestedLayout] })
defineProps({ user: Object })
</script>
<template>
  <h1>Welcome</h1>
  <p>Hello {{ user.name }}, welcome to your first Inertia app!</p>
</template>
```
در این حالت:
- ابتدا محتوای صفحه داخل `NestedLayout` قرار می‌گیرد.
- سپس کل ساختار درون `SiteLayout` نمایش داده می‌شود.
---
### نکته
با استفاده از `defineOptions({ layout: ... })` در Vue 3.3+ می‌توان به راحتی از **Persistent Layouts** بهره برد و تجربه کاربری روان‌تری فراهم کرد.
## Default layouts
### توضیح
اگر از **Persistent Layouts** استفاده می‌کنید، می‌توانید **Layout پیش‌فرض صفحات** را در فایل اصلی JavaScript برنامه خود تعریف کنید. این کار باعث می‌شود هر صفحه‌ای که Layout مشخصی ندارد، به‌طور خودکار داخل Layout پیش‌فرض قرار بگیرد.
### مثال ساده با Vue
```javascript
// This is the main JavaScript file where the default layout is set
import Layout from './Layout'
createInertiaApp({
  resolve: name => {
    const pages = import.meta.glob('./Pages/**/*.vue', { eager: true })
    let page = pages[`./Pages/${name}.vue`]
    // Set default layout if not already defined
    page.default.layout = page.default.layout || Layout
    return page
  },
  // ...
})
```
در این مثال:
- تابع `resolve()` برای بارگذاری صفحات استفاده می‌شود.
- اگر صفحه‌ای قبلاً Layout نداشته باشد، Layout پیش‌فرض (`Layout.vue`) به آن اختصاص داده می‌شود.
---
### مثال پیشرفته با شرط
می‌توان Layout پیش‌فرض را بر اساس نام صفحه به‌صورت شرطی تنظیم کرد، مثلاً صفحات عمومی بدون Layout پیش‌فرض باشند:
```javascript
// This is the main JavaScript file where the default layout is set conditionally
import Layout from './Layout'
createInertiaApp({
  resolve: name => {
    const pages = import.meta.glob('./Pages/**/*.vue', { eager: true })
    let page = pages[`./Pages/${name}.vue`]
    // Set default layout only for non-public pages
    page.default.layout = name.startsWith('Public/') ? undefined : Layout
    return page
  },
  // ...
})
```
در این حالت:
- اگر نام صفحه با `Public/` شروع شود، هیچ Layout پیش‌فرضی اختصاص داده نمی‌شود.
- برای سایر صفحات، Layout پیش‌فرض اعمال خواهد شد.
---
### نکته
استفاده از **Default Layouts** باعث می‌شود نیازی به اختصاص Layout در تک‌تک صفحات نباشد و مدیریت Layoutهای پایدار راحت‌تر شود.