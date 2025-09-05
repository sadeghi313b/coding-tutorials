# directions
| Direction | Meaning                                      | Examples       |
| --------- | -------------------------------------------- | -------------- |
| **x**     | افقی (چپ و راست)                             | `mx-*`, `px-*` |
| **y**     | عمودی (بالا و پایین)                         | `my-*`, `py-*` |
| **s**     | شروع (برای چیدمان راست به چپ یا چپ به راست)  | `ms-*`, `ps-*` |
| **e**     | انتها (برای چیدمان راست به چپ یا چپ به راست) | `me-*`, `pe-*` |
| **t**     | بالا                                         | `mt-*`, `pt-*` |
| **b**     | پایین                                        | `mb-*`, `pb-*` |
| **r**     | راست                                         | `mr-*`, `pr-*` |
| **l**     | چپ                                           | `ml-*`, `pl-*` |

# Colors
 **$$ <utility> - <color> - <colorStep/opacity> || \ leaveInWhite,Black $$
Ex: bg-blue-50/10

| Utility        | Description                                           |
| -------------- | ----------------------------------------------------- |
| bg-*           | رنگ پس‌زمینه یک عنصر را تنظیم می‌کند                  |
| text-*         | رنگ متن یک عنصر را تنظیم می‌کند                       |
| decoration-*   | رنگ تزئینات متن (مانند خط زیر متن) را تنظیم می‌کند    |
| border-*       | رنگ حاشیه یک عنصر را تنظیم می‌کند                     |
| outline-*      | رنگ خط محیطی یک عنصر را تنظیم می‌کند                  |
| shadow-*       | رنگ سایه‌های جعبه یک عنصر را تنظیم می‌کند             |
| inset-shadow-* | رنگ سایه‌های داخلی جعبه یک عنصر را تنظیم می‌کند       |
| ring-*         | رنگ سایه‌های حلقه‌ای یک عنصر را تنظیم می‌کند          |
| inset-ring-*   | رنگ سایه‌های حلقه‌ای داخلی یک عنصر را تنظیم می‌کند    |
| accent-*       | رنگ تأکید (accent) کنترل‌های فرم را تنظیم می‌کند      |
| caret-*        | رنگ مکان‌نما (caret) در کنترل‌های فرم را تنظیم می‌کند |
| fill-*         | رنگ پرشدگی (fill) عناصر SVG را تنظیم می‌کند           |
| stroke-*       | رنگ خط (stroke) عناصر SVG را تنظیم می‌کند             |

| color        | توضیحات                                                                                                                                                                                                                                                                                                  |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -inherit     | مقدار inherit باعث می‌شود که رنگ یک ویژگی  از والد (Parent) عنصر به ارث برده شود. اگر والد رنگی برای آن ویژگی تعریف نکرده باشد، این ارث‌بری به والدهای بالاتر ادامه می‌یابد تا به ریشه (مثل :root یا html) برسد.                                                                                         |
| -current     | مقدار current (یا currentColor) باعث می‌شود که رنگ یک ویژگی (مثل border-color یا outline-color) از رنگ متن فعلی عنصر (مشخص‌شده با color) گرفته شود. اگر color به‌صراحت تعریف نشده باشد، ممکن است از والد به ارث برده شود.                                                                                |
| -transparent | مقدار transparent رنگ را کاملاً شفاف (بدون رنگ) تنظیم می‌کند، به‌طوری که پس‌زمینه یا محتوای زیرین عنصر قابل‌مشاهده می‌شود. معادل rgba(0, 0, 0, 0) است (رنگ مشکی با شفافیت کامل). بر خلاف opacity که کل عنصر را شفاف می‌کند، transparent فقط رنگ ویژگی موردنظر (مثل background یا border) را شفاف می‌کند. |
| -\<color>    | red, blue, white, gray, ....                                                                                                                                                                                                                                                                             |

| colorStep: | 50, 100 ... 900, 950 |                        |
| ---------- | -------------------- | ---------------------- |
| opacity:   | 10 ... 100           | (--var) \|\| \[71.37%] |

# Background
همه یوتیلیتی های background با bg شروع می شوند.
## Gradiant
| -   | bg- | linear |                                         | - \<angle> \|\| (--var) \|\| [value] |
| --- | --- | ------ | --------------------------------------- | ------------------------------------ |
|     | bg- | radial | -to - { t,tr,tl \|\| b,br,bl \|\| r,l } | - \<angle> \|\| (--var) \|\| [value] |
| -   | bg- | conic  |                                         | - \<angle> \|\| (--var) \|\| [value] |
$$from || via || to - \frac{<color>}{<percentage>} || \frac{(--var)}{<angle>}$$
Ex:`from-10% via-30%  to-90%`
## Image
$$bg - \frac{[url(/img/mypic.jpg)]}{(image:(--filePath-var>))}$$
# Border / Outline
- ویژگی border یک حاشیه واقعی دور عنصر ایجاد می‌کند که بخشی از مدل جعبه (Box Model) است و روی ابعاد کلی عنصر تأثیر می‌گذارد.
- ویژگی outline یک خط خارجی دور عنصر ایجاد می‌کند که خارج از مدل جعبه قرار دارد و روی ابعاد یا چیدمان عنصر تأثیر نمی‌گذارد. یعنی ضخامت outline به ابعاد عنصر اضافه نمی‌شود و فضای اضافی اشغال نمی‌کند. چون outline روی مدل جعبه تأثیر نمی‌گذارد، برای افکت‌های موقتی (مثل هاور یا فوکوس) مناسب‌تر است.
- می‌توانید border و outline را همزمان استفاده کنید.

|         ویژگی          |                 Border                 |              Outline               |
| :--------------------: | :------------------------------------: | :--------------------------------: |
|    **محل قرارگیری**    |    داخل مدل جعبه، دور محتوا و پدینگ    |   خارج از مدل جعبه، دور کل عنصر    |
|   **تأثیر بر ابعاد**   |     روی ابعاد عنصر تأثیر می‌گذارد      |    روی ابعاد عنصر تأثیری ندارد     |
| **سفارشی‌سازی طرف‌ها** |  می‌توان هر طرف را جداگانه تنظیم کرد   |    فقط دور کل عنصر اعمال می‌شود    |
| **پشتیبانی از radius** | با `border-radius` گوشه‌ها گرد می‌شوند |   گوشه‌های گرد را دنبال نمی‌کند    |
|    **کاربرد اصلی**     |     حاشیه‌های دکوراتیو یا ساختاری      |   برجسته‌سازی یا افکت‌های فوکوس    |
|   **فاصله از عنصر**    |        نمی‌توان فاصله تنظیم کرد        | با `outline-offset` قابل تنظیم است |
## width
$$ \frac{border}{outline} - \frac{<direction>}{leaveForAllDirections} - \frac{leaveFor1px}{<pxNumber> ,  [2vw]  , (length:--var)} $$
$$ parent\cdot\ divide - < x || y > -  \frac{leaveFor1px}{<pxNumber> ,  [2vw]  , (length:--var)} $$
```
<div class="border-x-4 border-indigo-500 ..."></div>
```
## color
$$ \frac{border}{outline} - <color> $$
## style
$$ \frac{border||divide}{outline} - <style> $$
**style**: `solid, dashed, dotted, double, none. hidden`
## Border-radius
$$ rounded-\frac{<direction>}{<dir1><dir2>}-\frac{<size>||\ [val],\small(--var)}{none||full} $$

|Class|Left-to-right|Right-to-left|
| --- | --- | --- |
|`rounded-s-*`|`rounded-l-*`|`rounded-r-*`|
|`rounded-e-*`|`rounded-r-*`|`rounded-l-*`|
|`rounded-ss-*`|`rounded-tl-*`|`rounded-tr-*`|
|`rounded-se-*`|`rounded-tr-*`|`rounded-tl-*`|
|`rounded-es-*`|`rounded-bl-*`|`rounded-br-*`|
|`rounded-ee-*`|`rounded-br-*`|`rounded-bl-*`|

**size**: `xs, sm, md, lg, xl ... 4xl`
## Outline-offset
$$ \lceil-\rfloor outline-offset-\frac{<pxNumber>}{[val]\ ||\ (\tiny--\large var)} $$

Make a note of something, [[create a link]], or try [the Importer](https://help.obsidian.md/Plugins/Importer)!

When you're ready, delete this note and make the vault your own.