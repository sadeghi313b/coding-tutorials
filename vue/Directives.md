
دایرکتیو ها در اتریبیوتهای درون تگهای html به کار میروند. کار یک دایرکتیو اعمال کردن تغییرات به DOM بصورت reactive می‌باشد وقتی که مقدار عبارت آن تغییر می‌کند.
![[Pasted image 20250823174458.png]]
```vue ln=false
<script setup>
	const objectOfAttrs = { 
		id: 'container', 
		class: 'wrapper', 
		style: 'background-color:green' 
	}
</script>

<template>
	<p>Using text interpolation: { { rawHtml } } </p> 
	<p>Using v-html directive: <span v-html="rawHtml"></span></p>
	
	<div v-bind:id="dynamicId"></div>
	<div :id="dynamicId"></div>
	
	<div :id="id"></div>
	<div :id></div>
	
	<button :disabled="isButtonDisabled">Button</button>
	<div v-bind="objectOfAttrs"></div>
	
	# JavaScript Expressions
	// An expression is a piece of code that can be evaluated to a value. 
	// A simple check is whether it can be used after `return`
	<div :id="`list-${id}`"></div>
	{ { number + 1 } } 
	{ { ok ? 'YES' : 'NO' } } 
	{ { message.split('').reverse().join('') } }
	
	# Calling Functions
	<time :title="toTitleDate(date)" :datetime="date"> 
		{ { formatDate(date) } } 
	</time>
	
	# Dynamic Arguments {{with jsExpressions inside brackets}}
	<a :[attributeName]="attributeValue"> ... </a>
	<a @[eventName]="doSomething"> ... </a>
	<a :[someAttr]="value"> ... </a> نباید حروف بزرگ داشته باشد
	چون درون اچ تی ام ال حروف بزرگ بی معناست
	
</template>
```
# Conditional Rendering
```html ln=false
<button @click="condition = !condition">Toggle</button>

<h1 v-if="condition">Vue is awesome!</h1>
<h1 v-else>Oh no 😢</h1>


<div v-if="type === 'A'"> A </div>
<div v-else-if="type === 'B'"> B </div>
<div v-else-if="type === 'C'"> C </div>
<div v-else> Not A/B/C </div>
```
## multi tags
``` html ln=false
<template>
    <h1>App Component</h1>

    <template v-if="condition">
      <p>Detail 1</p>
      <p>Detail 2</p>
    </template>
</template>
```
خروجی اگر شرط درست باشد آنگاه فقط تگ h1 و دو تگ p است.
خروجی اگر شرط غلط باشد فقط تگ h1 است.
اگر به جای تمپلیت داخلی از div استفاده میشد آنگاه خروجی در هنگامیکه شرط درست باشد شامل یک div هم میشد.
سایر دایرکتیوها را نیز با استفاده از \<template> میتوان روی چندین تک اعمال کرد.
## v-if با v-for
وقتی که `v-if` و `v-for` هر دو روی یک عنصر استفاده شوند، ابتدا `v-if` ارزیابی می‌شود. این بدان معناست که شرط `v-if` دسترسی به متغیرهای درون اِسکوپ `v-for` نخواهد داشت. این مشکل با انتقال `v-for` به لایه بیرونی تر به یک تگ `<template>` (که واضح‌تر هم هست) حل می‌شود.
# List Rendering
```html ln=false
<script setup>
	const items = ref ([
		{ message: 'Foo' }, 
		{ message: 'Bar' }
	])
</script>

<template>
	<li v-for="item in items"> {{ item.message }} </li>
	<li v-for="(item, index) in items">
	  {{ index }} - {{ item.message }}
	</li>
	
	<!-- destructuring تخریب -->
	<li v-for="{ message } in items"> {{ message }} </li>
	<li v-for="({ message }, index) in items"> 
		{{ message }} {{ index }} 
	</li>
</template>
```
- برای `v-for` تو در تو، اِسکوپ متغیر هم مشابه توابع تو در تو عمل می‌کند. هر اِسکوپ `v-for` دسترسی به اِسکوپ والد و بالاتر دارد.
- می‌توانید به جای `in` از `of` به عنوان جداکننده استفاده کنید تا به سینتکس جاوااسکریپت برای iterator نزدیک‌تر باشد. \<div v-for="item of items">\</div>
## Loop on objects
```vue ln=false
<script setup>
	const myObject = reactive({
	  title: 'How to do lists in Vue',
	  author: 'Jane Doe',
	  publishedAt: '2016-04-10'
	})
</script>

<template>
	<li v-for="value in myObject"> 
		{ { value } } 
	</li>
	
	<li v-for="(value, key) in myObject">
	  { { key } }: { { value } }
	</li>
	
	<li v-for="(value, key, index) in myObject">
	  { { index } }. { { key } }: { { value } }
	</li>
</template>
```
## v-for with a Range
از یک شروع میشود نه از صفر. 
```vue ln=false
<span v-for="n in 10">{{ n }}</span>
```
## multi tags
هر جا **DOM عنصر وابسته به داده‌های پویا و حالت داخلی داشته باشد** (مثل input، select، textarea، کامپوننت‌ها)، **استفاده از key ضروری است** تا تغییرات لیست باعث خراب شدن مقادیر یا وضعیت داخلی نشود.
```vue ln=false
<div v-for="item in items" :key="item.id">
  <!-- content -->
</div>

<template v-for="todo in todos" :key="todo.name">
  <li>{{ todo.name }}</li>
</template>
```
## v-for with a Component
```vue ln=false
<MyComponent
  v-for="(item, index) in items"
  :item="item"
  :index="index"
  :key="item.id"
/>
```
می‌توانید مستقیما `v-for` را روی یک کامپوننت، مانند هر المان عادی، استفاده کنید (فراموش نکنید `key` ارائه دهید). اما این به طور خودکار داده‌ای را به کامپوننت منتقل نمی‌کند، چرا که کامپوننت‌ها اِسکوپ مستقل خودشان را دارند. برای منتقل کردن داده‌ به کامپوننت، باید از props هم استفاده کنیم.
# Form Input Bindings
دایرکتیو v-model در انواع مختلف المانهای ورودی پذیر html مانند input, textarea, select برای اتصال بین یک متغیر با element value استفاده میشود.
```vue ln=false
<input v-model="myvar">
<input type="checkbox" v-model="checked">
<textarea v-model="message" placeholder="add multiple lines"></textarea>

<div>
    <input type="radio" value="Male" v-model="gender"> Male
    <input type="radio" value="Female" v-model="gender"> Female
    <p>Gender: {{ gender }}</p>
</div>
```

```vue ln=false
<input type="checkbox" v-model="toggle" 
	true-value="yes" false-value="no" />

<input type="checkbox" v-model="toggle" 
	:true-value="dynamicTrueValue" :false-value="dynamicFalseValue" />
```
در واقع `true-value` و `false-value` ویژگی‌های مخصوص Vue هستند که فقط با `v-model` کار می‌کنند. با علامت زدن کادر روی `'yes'` و وقتی علامت آن را بردارید روی `'no'` تنظیم می شود.

```vue ln=false	
<input type="radio" v-model="pick" :value="first" /> 
<input type="radio" v-model="pick" :value="second" />
```
وقتی اولین ورودی رادیویی علامت زده می‌شود، متغیر `pick` روی مقدار `first` تنظیم می‌شود و وقتی ورودی دوم انتخاب می‌شود روی مقدار `second` تنظیم می‌شود.

```vue ln=false
<select v-model="selected">
	<option :value="{ number: 123 }">123</option> 
</select>
```
اینجا `v-model` از اتصال داده مقادیر غیر رشته‌ای نیز پشتیبانی می‌کند! در مثال بالا هنگامی که option انتخاب شده است، `selected` به آبجکت `{ number: 123 }` تنظیم می‌شود.

```vue ln=false
<input v-model.lazy="msg" />
<input v-model.number="age" />
<input v-model.trim="msg" />
```
# Ref
دسترسی به المانی از DOM در اسکریپت:
```vue ln=false
<script setup> 
	import { useTemplateRef, onMounted } from 'vue' 
	// در تمپلیت مطابقت داشته باشد ref آرگومان اول باید با مقدار 
	const iinput = useTemplateRef('my-input') 
	const childRef = useTemplateRef('child')
	onst itemRefs = useTemplateRef('items')
	
	onMounted( () => { 
		iinput.value.focus() 
		// childRef.value will hold an instance of <Child />
		console.log(itemRefs.value)
	}) 
</script> 
<template> 
	<input ref="my-input" /> 
	<Child ref="child" />
	
	<li v-for="item in list" ref="items"> {{ item }} </li>
	
	<input :ref="(el) => { /* assign el to a property or ref */ }">

</template>
```