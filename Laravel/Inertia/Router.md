| Method            | HTTP Verb                               | Typical Use Case                        | Example                                                                               | Notes                                                            |
| ----------------- | --------------------------------------- | --------------------------------------- | ------------------------------------------------------------------------------------- | ---------------------------------------------------------------- |
| **router.get**    | `GET`                                   | گرفتن داده، فیلتر، جستجو، پگینیشن       | `router.get('/students', { search: 'ali' }, { preserveState: true })`                 | ساده‌ترین و رایج‌ترین برای query string.                         |
| **router.post**   | `POST`                                  | ایجاد رکورد جدید (فرم create)           | `router.post('/students', { name: 'Ali', email: 'ali@example.com' })`                 | شورت‌کات برای `router.visit` با method `POST`.                   |
| **router.put**    | `PUT`                                   | آپدیت یک رکورد                          | `router.put('/students/1', { name: 'Updated' })`                                      | شورت‌کات برای `router.visit` با method `PUT`.                    |
| **router.delete** | `DELETE`                                | حذف رکورد                               | `router.delete('/students/1')`                                                        | شورت‌کات برای `router.visit` با method `DELETE`.                 |
| **router.visit**  | Custom (`GET`, `POST`, `PUT`, `DELETE`) | استفاده پیشرفته، کنترل کامل روی request | `js router.visit('/students', { method: 'post', data: {...}, preserveScroll: true })` | انعطاف‌پذیرترین متد؛ می‌توان همه نوع درخواست را با آن انجام داد. |

### router.get
- مخصوص درخواست‌های **GET** هست.
- سینتکس ساده‌تری برای فیلتر/جستجو/پگینیشن.
- وقتی می‌خوای فقط دیتا و state صفحه رو آپدیت کنی بدون اینکه درخواست POST/PUT/DELETE بزنی.
### router.visit
- **متد عمومی‌تر** هست و می‌تونی نوع درخواست رو خودت تعیین کنی (`GET`, `POST`, `PUT`, `DELETE`).
- مناسب برای فرم‌ها یا اکشن‌هایی که نیاز به متد غیر از GET دارن.
### Summery
- اگر فقط می‌خوای صفحه‌ای با query string (مثل search, filters, pagination) آپدیت بشه → `router.get` استفاده کن.
- اگر می‌خوای **فرمی submit بشه یا تغییری در دیتابیس** ایجاد کنی → `router.visit` استفاده کن (با method دلخواه).