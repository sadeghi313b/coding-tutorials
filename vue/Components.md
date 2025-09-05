


```vue ln=false title=parent 
<script setup> 
	import ChildComponent from '/path/to/component' 
	
</script> 

<template> 
	<h1>parent</h1> 
	<ChildComponent />
	<ChildComponent></ChildComponent> 
	<child-component></child-component>
	<child-component />  <!-- Error -->
</template>
```
### Global Components
می‌توان یک [کامپوننت را به طور سراسری ثبت کرد](https://fa.vuejs.org/guide/components/registration.html) که آن را بدون نیاز به import کردن، در دسترس تمام کامپوننت‌های یک برنامه داشته باشیم.
### استقلال داده ها در فراخوانی ها
در Vue هر بار که یک کامپوننت را فراخوانی می‌کنید، یک **نمونه جدید** از آن ساخته می‌شود. این یعنی متغیرها و داده‌های داخلی آن کامپوننت مخصوص همان نمونه هستند و هیچ ارتباطی با مقادیر همان متغیرها در نمونه‌های دیگر ندارند؛ بنابراین هر بار استفاده از یک کامپوننت، مانند ساختن یک محیط تازه و مستقل است.
# Property Binding
```vue ln=false title=خلاصه
<!-- component script -->
<script setup>
	const childprop = defineProps(['customAtt2'])
	defineModel('name', String)
</script>

<!-- parent template -->
<template>
	<Component ~ :customAtt = "expression" :customAtt2 = "expression" ~ ~ />
	<Component ~ v-model:name ="expression" v-model:someNum ="expression" ~ ~ />
</template>
```
## ارسال پراپرتی به کامپوننت
وقتی در والد یک کامپوننت فراخوانی می شود، میتوان داده هایی را به وسیله attribut ها به کامپوننت ارسال کرد. اینکار هم از طریق v-binding (دو نقطه) و هم بدون آن ممکن است. یک نام دلخواه به اتریبیوت می دهیم و همان نام را در کامپوننت به عنوان پراپ تعریف می کنیم.
ParentCustomAttributes-- passingData ---ComponentProps

راه دیگر ارسال obj با <span dir=ltr>v-bind="objName"</span> در اتریبیوتِ تگِ کامپوننت در والد است. v-bind تمام کلیدهای آبجکت را باز می‌کند و به صورت prop های جداگانه به فرزند می‌فرستد. بنابراین در کامپوننت باید آن را 

```vue ln=false title=Parent
<script setup>
	~
	const post = { 
		prop1: 1, 
		prop2: 'My Journey with Vue' 
	}
</script>
<template>
	<Component ~ :customAtt = "expression" :customAtt2 = "expression" ~ ~ />
	
	<Component ~  customAtt = "false" ~ /> // false consumed as string
	<Component ~ :customAtt = "false" ~ /> // false consumed as expression = bool
	
	<Component ~ is-published ~ /> // pass isPublished=true to childComponent 
	
	<BlogPost v-bind="post" /> //ارسال مجموعه ای
</template>
```

```vue ln=false title=child 
<script setup>
	defineProps(['customAtt1'])
	console.log(customAtt1) // error: it must be assigned to a variable
	
	const childprop = defineProps(['customAtt2'])
	console.log(childprop.customAtt2)
	
	const { childprop } = defineProps(['customAtt3'])
	console.log(childprop)
	
	const props = defineProps(['prop1', 'prop2'])
	const props = defineProps({ 
		prop1: Number, 
		prop2: String 
	})
	console.log(props.prop2)
</script>

<template>
  <h4> { { customAtt1 } } </h4> //here is avalable without scriptAssigning to a variable
</template>
```
> توجه شود که در  <script setup نمی‌شود دو بار <span dir=ltr>defineProps()</span> نوشت. `<script setup>` به صورت compile-time کار می‌کند و فقط **یک فراخوانی `defineProps()` مجاز است**.
## ارسال از فرزند به والد و برعکس
برخلاف props  که برای تعریف propsهایی که از والد به کامپوننت ارسال می‌شوند استفاده می شود، در واقع v-model یک اتصال دو طرفه بین کامپوننت و والد به دست می دهد.

| defineProps() | parent → child |
| ------------- | -------------- |
| defineModel() | child ↔ parent |

```vue ln=false title=Parent
<template>
	<Component ~ v-model:name ="expression" v-model:someNum ="expression" ~ ~ />
</template>
```

```vue ln=false title=child 
<script setup>
	defineModel('name', String) یک متغیر محلی با همان نام ایجاد میشود
	const countInComponent = defineModel('someNum', Number)
	// syntax: defineModel('modelName', Type)
</script>

<template>
	<p>Child count: { { countInComponent } }</p>
	<button @click="countInComponent++">Increment Child Count</button>
	<input v-model="name" placeholder="Enter your name" />
</template>
```
**ارتباط دوطرفه**: وقتی در کامپوننت روی دکمه کلیک می‌کنیم، مقدار countInComponent تغییر می‌کند و Vue به صورت خودکار someNum را در والد به‌روز می‌کند. مشابه همین برای name در والد اتفاق می افتد.

اگر اتصال دو طرفه فقط شامل یک متغیر باشد میتوان از آرگومان دهی به defineModel چشم پوشی کرد:
```vue ln=false title=parent 
<!-- parent template -->
    <ChildComponent v-model="someParentProp-or-writableEpression"/>
    <!-- عبارت باید قابل نوشتن باشد و نه فقط خواندنی چون اتصال دو طرفه داریم -->
<!-- component script -->
	const childVar = defineModel()
```
# Event Binding
## ارسال رویداد از کامپوننت به والد
یعنی چه کار کنیم که والد به فراخوانی رویداد خودش از جانب فرزند گوش فرا دهد. عملکرد و فانکشن رویداد در والد درون تگ کامپوننت تعریف می شود. سپس رویدادی در کامپوننت، آن رویداد والد را صدا می زند. یعنی رویداد اصلی در والد تاسیس شده و و با یک نام شاخص aliasName توسط تگ کامپوننت به کامپوننت معرفی می شود. سپس کامپوننت در هر جایی از خودش از این نام شاخص استفاده میکند و رویداد اصلی را صدا می زند. پس والد به فراخوانی رویداد خودش از طرف فرزند گوش فرا میدهد.
- رویداد **اصلی** (هندلر) در والد تعریف می‌شود. مثلا (`handleHello`).
- این متد در تگ کامپوننتِ والد به صورت **event listener** یک نام شاخص پیدا میکند. مثلا در مثال زیر والد دکمه آتشِ این رویداد را از طریق نام شاخص myEvent در اختیار کامپوننت قرار میدهد.
<span dir=ltr> @myEvent="handlerFn-or-methodicExpression"</span>
- کامپوننت با defineEmits همان نام شاخص را میگیرد و حالا درون تمپلیتش میتواند با متد <span dir=ltr>$emit(aliasName)</span> آن را آتش کند. درون اسکریپتش نیز اگر به صورت زیر شناسانده شود آنگاه قابل استفاده است:
const myemit = defineEmits(\['myEvent'])

```vue ln=false title=parent 
<template>
	<Component ~ @aliasName = "someMethod()-or-methodicEpression" ~ />
</template>
```

```vue ln=false title=child 
<script setup>
	defineProps(['title'])
	defineEmits(['enlarge-text'])
	
	const myemit = defineEmits(['enlarge-text'])
	function handleClick() {
	    myemit('enlarge-text')  // ارسال رویداد به والد
	}
</script>

<template>
	<h1 class="blog-post" />
	<button @click="$emit('enlarge-text')">Enlarge text</button>
	<button @click="() => myemit('enlarge-text')">Enlarge text</button>
	<button @click="handleClick">Enlarge text</button>
</template>
```
# Pass Html : Slots
ارسال محتوای html از والد به فرزند. یعنی محتوای درونی توسط کامپوننت والد فراهم می‌شود.
محتوای اسلات فقط به یک متن ساده محدود نمی‌شود. می‌تواند هر محتوای معتبر تمپلیت باشد. برای مثال، می‌توانیم چندین المنت یا حتی چند کامپوننت‌ دیگر را ارسال کنیم. می توانیم از {{myvar}} استفاده کنیم. 
بنا بر یک اصل کلی: عبارات در تمپلیت والد فقط به اسکوپ والد دسترسی دارند و عبارات در تمپلیت فرزند فقط به اسکوپ فرزند دسترسی دارند. پس از محتوای والد به متغیر های فرزند دسترسی نداریم. 
اگر والد هیچگونه محتوایی ارائه نکرده باشد آنگاه شاید بخواهیم فرزند یک محتوای پیش فرض نشان دهد. 

| \<slot /> | اسلات ها درون کامپوننت قرار میگیرند و محتوای والد را درون فرزند قرار می دهند |
| --------- | ---------------------------------------------------------------------------- |
```vue ln=false title=parent 
<template>
	...
	<Child>
		<template #default>
			some html contents
		</template>
		some html contents
		
		<template v-slot:name1>
			some html contents
		</template>
		<template #name2>
			some html contents
		</template>
		<template #[dynamicSlotName]> some-html </template>
	</Child>
	<Child #name4>
		{{ name4.text }} {{ name4.count }}
	</Child>
	...
</template>
```

```vue ln=false title=child 
<template>
	...
	<slot /> 
		or 
	<slot> default content محتوای پیش فرض </slot>
		or
	<slot name="name1"></slot>
	<slot name="name2"></slot>
	
	<div v-if="$slots.name3"> 
		<slot name="name3" /> 
	</div>
	
	<slot :text="greetingMessage" :count="1"></slot> ارسال داده به والد
	...
</template>
```
# Fallthrough Attributes
اگر **نمی‌خواهید** یک کامپوننت به طور خودکار ویژگی‌ها را به ارث ببرد، می‌توانید `inheritAttrs: false` را در آپشن‌های کامپوننت تنظیم کنید.
```vue ln=false title=child 
<script setup>
	defineOptions({ 
		inheritAttrs: false 
	})
</script>

<template>
</template>
```
# Provide / Inject
معمولاً وقتی می‌خواهیم داده‌ای را از والد به فرزند منتقل کنیم، از [props](https://fa.vuejs.org/guide/components/props.html) استفاده می‌کنیم. اما حالتی را تصور کنید که کامپوننت‌های تو در توی زیادی داریم و یک کامپوننت در عمق درخت نیاز به داده‌ای از یک کامپوننت اجدادی با فاصله زیاد دارد.
# Async Components
در برنامه‌های بزرگ، ممکن است لازم باشد برنامه را به بخش‌های کوچکتر تقسیم کنیم و فقط در هنگام نیاز به یک کامپوننت، آن را از سرور بارگذاری کنیم.
