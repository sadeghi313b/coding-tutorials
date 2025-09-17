```vue ln=false title=some
<script setup>
	defineProps([prop1, prop2])
	const someProp = ref(someValue);
	const someReactive = reactive( {someKeys:someValues} )
	console.log(prop1) //no need to prop1.value
	console.log(someProp.value) //needs .value
	console.log(someReactive.someKeys)
</script>

<template>
</template>
```
# Reactivity
```vue ln=false
<script setup>
	import { ref,reactive, toRefs } from 'vue'
	const count = ref(0) 
	// داده را در یک آبجکت می پیچدوآن آبجکت را ریاکتیو می کند
	const state = reactive({ count: 0 }) //only
	// خود آبجکت را ریاکتیو میکند 
	const { prop1, prop2 } = toRefs(props); //keep of reactivity
	consol.log(prop1.value);
	const { prop3, prop4 } = props; //loose of reactivity
	consol.log(prop1);
</script>
```

|    ref()    |                               reactive()                               |                            |
| :---------: | :--------------------------------------------------------------------: | -------------------------- |
|  {{count}}  |                            {{state.count}}                             | دسترسی به متغیر در تمپلیت  |
| count.value |                              state.count                               | دسترسی به متغیر در اسکریپت |
|  all types  |                only collection types: array,obj,map,set                | نوع داده                   |
|  شدنی است   | ناشدنی است. <br>باید پراپرتی های داخل آبجکت تک تک مقدار دهی جدید شوند. | مقدار دهی مجدد کل آبجکت    |
# Computed
```vue ln=false
<script>
import { reactive, computed } from 'vue'

	const author = reactive({ 
		name: 'John Doe', 
		books: [ 
			'Advanced Guide', 
			'Basic Guide', 
			'The Mystery' 
		] 
	})
	
	const hasBookProp = computed(  () => {
		return author.books.length > 0 ? 'Yes' : 'No'
		}
	)
	function hasBookMethod() { 
		return author.books.length > 0 ? 'Yes' : 'No' 
	}
</script>
```
- دسترسی به myvar همانند ref است یعنی در اسکریپت با myvar.value و در تمپلیت بصورت {{hasBookProp}}.
- در اینجا myvar یک پراپرتی است نه متد. دسترسی به متد در تمپلیت: <span dir=ltr>{{ hasBookMethod() }}</span>
- یک پراپرتی computed تنها زمانی دوباره ارزیابی می‌شود که برخی از وابستگی‌های reactive آن تغییر کرده باشند. این بدان معناست که تا زمانی که `author.books` تغییر نکرده باشد، دسترسی به `publishedBooksMessage` نتیجه محاسبه قبلی را برمی‌گرداند، بدون نیاز به اجرای مجدد تابع getter. اما متد **همیشه** هر زمان که رندر مجدد اتفاق بیفتد، فراخوانی می‌شود.
- کاربرد: در جائیکه خروجی محاسبات وابسته به پراپرتی های reactive است و می‌خواهیم فقط **وقتی داده تغییر کرد** محاسبه انجام شود.مثلا در صفحه فروشگاه با هر بار رندر صفحه، مجموع قیمت محصولات مجددا محاسبه نشود و از computed cash خوانده شود.
- پراپرتی computed زیر هیچ وقت به‌روز نمی‌شود، زیرا `Date.now()‎` یک reactive نمی‌باشد.
```js ln=false
const now = computed(() => Date.now())
```
# Watchers
پراپرتی های computed به شما این امکان را می‌دهند که مقادیر جدید را براساس داده‌های موجود محاسبه کنید. با این حال در یک سری از موارد ما باید در واکنش به تغییرات، عملیات‌هایی را انجام دهیم. برای مثال تغییر در DOM یا تغییر بخشی از state که نتیجه یک عملیات همگام async است.

فرض کن در صفحه‌ی خرید یک فروشگاه، کاربر تعداد کالا را وارد می‌کند. اگر فقط بخواهیم قیمت کل را محاسبه کنیم آنگاه - نیازی به "کار اضافه" یا "اثر جانبی" نداری، فقط کافی است یک مقدار **مشتق‌شده** (derived value) محاسبه شود که تا اینجا **Computed** کافی است.
اما اگر بخواهی با تغییر کالا، ** یک درخواست به سرور بفرستی** تا موجودی انبار چک شود - این کار دیگر فقط محاسبه‌ی یک مقدار نیست، بلکه یک **اثر جانبی (side effect)** است: مثل ارسال درخواست Ajax، تغییر DOM یا اجرای یک تایمر. در این حالت باید از **watch** استفاده کنیم. بنابراین Watch برای زمانی است که نیاز داری به **تغییر یک داده واکنش نشان بدهی**م و یک **کار اضافی** (async, DOM, side effect) انجام دهیم.

| --       | --            |                                                              |
| -------- | ------------- | ------------------------------------------------------------ |
| watch    | side effects  | دو آرگومان ورودی: newval و oldval را خود ویو مقدار دهی میکند |
| computed | derived value | آرگومان ورودی نمی گیرد                                       |
```vue ln=false
<script setup>
	watch ( watchedVar, (newval,oldval) => { //not watchedVar.value
		//do something
	})
	
	newval = computed ( () => {
		return jsExpression-included-derived-vars
	})
</script>
```
---
# Reactivity
با سلام
این یک نکته خیلی مهم در Vue 3 و **reactivity system** است که تفاوت بین `ref([...])` و `computed(() => ...)` را روشن می‌کند.

### 1️⃣ روش اول: `const rows = ref([...props.users.data]);`
```js
const rows = ref([...props.users.data]);
```
- اینجا `[...props.users.data]` یک **کپی سطحی (shallow copy)** از آرایه اصلی ساخته است.
- کپی ساخته شده **یک snapshot ثابت** از زمان mount کامپوننت است.
- بعد از آن، اگر `props.users.data` در اثر یک **درخواست جدید از سرور** (مثلاً بعد از حذف رکورد یا صفحه‌بندی) تغییر کند، **کپی ساخته شده تغییر نمی‌کند**.
- به عبارت دیگر، این آرایه مستقل شده و reactive نسبت به تغییر `props.users.data` نیست.
در نتیجه، جدول **به‌روزرسانی نمی‌شود** چون `rows.value` دیگر به `props.users.data` وصل نیست.
---
### 2️⃣ روش دوم: `const rows = computed(() => props.users.data || []);`
```js
const rows = computed(() => props.users.data || []);
```
- اینجا `rows` **یک computed reactive** است که هر بار `props.users.data` تغییر کند، مقدار جدید را محاسبه می‌کند.
- مزیت: **همیشه با props اصلی سینک است** و تغییرات backend (مثلاً حذف رکوردها یا صفحه‌بندی جدید) روی جدول منعکس می‌شود.
- در واقع جدول به جای استفاده از snapshot، همیشه **به منبع داده واقعی** نگاه می‌کند.
---
### ✅ جمع‌بندی
|روش|آیا reactive نسبت به props است؟|توضیح|
|---|---|---|
|`ref([...props.users.data])`|❌ نه|فقط یک کپی سطحی مستقل، تغییرات props روی آن اثر ندارد|
|`computed(() => props.users.data||[])`|

💡 نکته عملی:
- وقتی داده‌ها از **props** می‌آیند و ممکن است تغییر کنند، همیشه بهتر است از **computed** استفاده کنید.
- `ref([...])` بیشتر زمانی کاربرد دارد که بخواهید یک آرایه داخلی کاملاً مستقل داشته باشید و تغییر آن هیچ ارتباطی با props نداشته باشد.
---
اگر بخواهی، می‌توانم **روش درست حذف ردیف‌ها و بروز رسانی جدول بدون رفرش صفحه** را هم با `computed` و Inertia نشان بدهم، طوری که انتخاب و حذف رکوردها درست کار کند.
می‌خوای این کار را انجام دهم؟