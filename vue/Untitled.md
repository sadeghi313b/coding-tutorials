# method () \_vs_ method

| @click="deleteRecords()" | تابع در هنگام رندر ابتدایی html بدون کلیک فراخوانی و اجرا می‌شود |
| ------------------------ | ---------------------------------------------------------------- |


# Class and Style Bindings
درون تگهای html، به اتریبیوت ها class و style علاوه بر متغیرهای رشته ای، میتوان آبجکت ویا آرایه را نیز متصل کرد.
کنیم:
```html ln=false
<script setup>
const classObject = reactive({ 
	active: true, 
	'text-danger': false 
})
//or
const classObject = computed(   () => ({ 
	active: isActive.value && !error.value, 
	'text-danger': error.value && error.value.type === 'fatal' 
}))
</script>

<template>
	//object
	<div :class="{ active: isActive, 'text-danger': hasError }"></div>
	<div :class="classObject"></div>
	
	//array
	<div :class="[class1, class2, class3]"></div> 
	<div :class="[condition ? class1 : '', class3]"></div>
	<div :class="[ {class1:condition}, class2]"></div>
	<div :class="[ {[myclass_var]:condition}, class2]"></div>
</template>
```
سینتکس بالا بدین معناست که وجود کلاس `active` توسط درستی ([Truthy](https://developer.mozilla.org/en-US/docs/Glossary/Truthy)) داده `isActive` تعیین خواهد شد.
# Composables
```javascript ln=false title=composable:useExample.js

import { ref, computed } from 'vue' 
export function useExample() { //convention:use+funName: camelCase
	// reactive state 
	const count = ref(0) 
	// computed properties 
	const doubleCount = computed( () => count.value * 2 ) 
	// methods function increment() { count.value++ } 
	// expose state and methods 
	return { count, doubleCount, increment } 
}
```

```vue ln=false title=app 
<script setup>
	import { useExample } from './useExample.js'
	const { count, doubleCount, increment } = useExample()
</script>

<template>
  <div>
    <p>Count: {{ count }}</p>
    <p>Double: {{ doubleCount }}</p>
    <button @click="increment">Increment</button>
  </div>
</template>
```
# Transition
```vue ln=false
<script setup>
	import { ref } from 'vue'
	const show = ref(true)
</script>
<template>
	<button @click="show = !show">Toggle</button>
	
  <transition
    name="fade"    پیشوند کلاس‌های سی اس اس
    mode="out-in"  ترتیب خروج و ورود به ترنزیشن
    appear   رویداد گرایی برای اجرای کدهای جاوا در مراحل مختلف
    @before-enter="beforeEnter"
    @enter="enter"
    @after-enter="afterEnter"
    @before-leave="beforeLeave"
    @leave="leave"
    @after-leave="afterLeave"
  >
	    <div v-if="show">Hello Vue Transition</div>
  </transition>
</template>
```

\<transition followingAttributes=value> ... \</transition>

| Attribute / Prop | نوع           | توضیح                                                    |
| ---------------- | ------------- | -------------------------------------------------------- |
| name             | String        | پیشوند کلاس CSS برای transition                          |
| appear           | Boolean       | فعال کردن transition هنگام initial render                |
| css              | Boolean       | استفاده یا عدم استفاده از کلاس‌های CSS (true by default) |
| type             | String        | `transition` یا `animation` برای تعیین نوع انیمیشن       |
| mode             | String        | `in-out` یا `out-in` برای ترتیب mount/unmount            |
| duration         | Number/Object | مدت زمان انیمیشن (می‌تواند object با enter/leave باشد)   |
Event hooks (جاوااسکریپتی)
| Event           | توضیح                                 |
|-----------------|---------------------------------------|
| before-enter    | قبل از شروع enter                     |
| enter           | هنگام شروع enter، امکان اجرای JS دارد|
| after-enter     | بعد از اتمام enter                     |
| enter-cancelled | اگر enter کنسل شد                     |
| before-leave    | قبل از شروع leave                     |
| leave           | هنگام شروع leave                       |
| after-leave     | بعد از اتمام leave                     |
| leave-cancelled | اگر leave کنسل شد                     |

### کلاس‌های CSS که Vue به طور خودکار اضافه می‌کند

| فاز          | کلاس اضافه شده   |
| ------------ | ---------------- |
| قبل از enter | `v-enter-from`   |
| هنگام enter  | `v-enter-active` |
| بعد از enter | `v-enter-to`     |
| قبل از leave | `v-leave-from`   |
| هنگام leave  | `v-leave-active` |
| بعد از leave | `v-leave-to`     |
اگر `name="fade"` تنظیم شود، پیشوند `v-` با `fade` جایگزین می‌شود: (و مشابه برای leave)
- `fade-enter-from`
- `fade-enter-active`
- `fade-enter-to`
