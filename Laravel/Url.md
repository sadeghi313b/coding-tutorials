### مثال full URL

فرض کنید کاربر به صفحه زیر مراجعه کرده است:

```
https://example.com/users/42/edit?tab=profile&ref=dashboard
```

در لاراول، اگر از متد `$request->fullUrl()` استفاده کنید، خروجی دقیقاً همین URL خواهد بود:

```php
// Example: using fullUrl()
$request->fullUrl(); 
// Output: https://example.com/users/42/edit?tab=profile&ref=dashboard
```

### جدول تجزیه بخش‌های URL

| URL part     | مقدار در مثال               | نام انگلیسی                      |
| ------------ | --------------------------- | -------------------------------- |
| Scheme       | `https`                     | Scheme / Protocol                |
| Host         | `example.com`               | Host / Domain                    |
| Port         | (پیش‌فرض 443 برای HTTPS)    | Port                             |
| Path         | `/users/42/edit`            | Path                             |
| Path segment | `edit`                      | Path segment / Action / Endpoint |
| Query        | `tab=profile&ref=dashboard` | Query / Query string             |
| Fragment     | (ندارد در این مثال)         | Fragment / Anchor                |