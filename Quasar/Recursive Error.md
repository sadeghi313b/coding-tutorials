با سلام

الان موضوع روشن شد 🚀  
خطای `Maximum recursive updates exceeded in component <QVirtualScroll>` به خاطر اینه که تو داری روابط (relations) رو هم از لاراول پاس میدی.

### چرا این مشکل پیش میاد؟

وقتی `with(['phones','roles','employee:id,user_id'])` می‌دی، لاراول توی خروجی `users.data` علاوه بر فیلدهای ساده، **آبجکت‌های تو در تو (nested objects)** هم می‌سازه.

اما `QTable` (وقتی `virtual-scroll` فعاله) فرض می‌کنه هر ردیف (`row`) یک **آبجکت مسطح (flat object)** باشه. وقتی آبجکت تو در تو می‌بینه، Vue سعی می‌کنه اون رو رندر کنه → واکنش‌پذیری عمیق (deep reactivity) → لوپ بی‌پایان.

---

### ✅ راه‌حل‌ها

#### 1. خروجی را تخت (flatten) کن

قبل از پاس دادن به `QTable`، روابط را به شکل ساده دربیار. مثلاً در فرانت:

```js
const rows = computed(() => {
  return props.users.data.map(user => ({
    ...user,
    phone_numbers: user.phones?.map(p => p.number).join(', ') || '',
    role_names: user.roles?.map(r => r.name).join(', ') || '',
    employee_code: user.employee?.id || '',
  }))
})
```

و بعد توی `q-table`:

```vue
<q-table
  :rows="rows"
  :columns="columns"
  row-key="id"
  ...
/>
```

---

#### 2. یا در لاراول، DTO / Resource استفاده کن

بهتره از **Resource** استفاده کنی و خودت خروجی رو مسطح بسازی:

```php
return Inertia::render('Users/Index', [
    'users' => UserResource::collection($paginator),
]);

// UserResource.php
public function toArray($request)
{
    return [
        'id' => $this->id,
        'name' => $this->name,
        'email' => $this->email,
        'phones' => $this->phones->pluck('number'),
        'roles' => $this->roles->pluck('name'),
        'employee_code' => $this->employee?->id,
    ];
}
```

---

#### 3. موقتاً virtual-scroll را غیرفعال کن

اگر flatten نکردی، می‌تونی برای تست فقط `virtual-scroll` رو برداری و خطا رفع میشه.  
ولی راه اصولی → flatten data (روش 1 یا 2).

---

📌 جمع‌بندی:

- علت: داده‌های تو در تو (relations) + virtual-scroll.
    
- راه‌حل: یا توی Vue داده‌ها رو مسطح کن، یا توی لاراول با Resource خروجی رو کنترل کن.
    

---

می‌خوای من برات یک نسخه **بازنویسی‌شده از rows** توی `Index.vue` بنویسم که خودش اتومات روابط مثل phones و roles رو به رشته تبدیل کنه و آماده برای `QTable` بشه؟