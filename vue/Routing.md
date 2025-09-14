# Client-Side vs. Server-Side Routing
در **Client-Side Routing** (Vue Router, React Router)، فقط کامپوننت مثلا <span dir=ltr>/about</span> جایگزین می‌شود. نه اینکه سرور یک فایل مثل about.blade.php را رندر و به مرورگر ارسال کند. 
This allows Vue Router to change the URL without reloading the page.

در یک پروژه laravel+inertia:
- - مسیرها هنوز در لاراول تعریف می‌شوند (SEO-friendly).
- ولی رفرش کامل صفحه رخ نمی‌دهد (SPA-like).

برای **Client-Side Routing** در مرورگر:
 1. روش History API: 
`history.pushState(state, title, url)` → <span dir="rtl">یک URL جدید در history مرورگر اضافه می‌کند.</span>
`history.replaceState(state, title, url)` → URL فعلی را جایگزین می‌کند.
`popstate event` → را منتشر می‌کند وقتی کاربر روی دکمه Back/Forward کلیک می‌کند.
2. متد <span dir="ltr">haschange()</span> : استفاده از بخش **hash (#)** در URL برای تغییر مسیر بدون رفرش.

two components provided by Vue Router, `RouterLink` and `RouterView`:
```javascript ln=false title=
<RouterLink to="/">Go to Home</RouterLink> //Instead of using regular `<a>`
<RouterView />
```

```vue ln=false title=App.vue
<template>
  <h1>Hello App!</h1>
  <p><strong>Current route path:</strong> { { $route.fullPath } }</p>
  <nav>
    <RouterLink to="/">Go to Home</RouterLink>
    <RouterLink to="/about">Go to About</RouterLink>
  </nav>
  <main>
    <RouterView />
  </main>
</template>
```
کامپوننت `<RouterView>` نقش **placeholder** را در app.vue دارد. یعنی هر بار که مسیر (route) تغییر می‌کند، Vue Router بررسی می‌کند کدام **component** باید نمایش داده شود و آن را دقیقاً در محل `<RouterView>` رندر می‌کند.