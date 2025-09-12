# cmd
چگونه از windows explorer وارد command shell شویم:
1. تایپ cmd در نوار آدرس
2. شیفت + کلیک راست در فضای خالی از پوشه و انتخاب open powershell
# vue (noStarterKit)
\>> laravel new \<project-name>
. . .
\>> cd \<project-name>
\>> npm $\frac{install}{i}$ vue@latest

---
# npm's
## ziggy
استفاده از روتهای لاراول در سمت فرانت
npm install ziggy-js

add `@routes` to app.blade.php befor \</head> tag

//ziggy in app.js
import { ZiggyVue } from 'ziggy-js'
import { Ziggy } from './ziggy'

---
# vs-code from cmd
\>> code .
# client-side
## inertiajs.com
>**inertiajs.com -> server-side -> install dependencies: **
\>> composer require inertiajs/inertia-laravel

## Root template
resources/views/welcome.blade.php `->change->` app.blade.php 
### html boiler plate
\> !
### Middleware
after appending `HandleInertiaRequests` middleware to the `web` middleware group in your application's `bootstrap/app.php` file, please add it into tne Namespace by rightClick on it and then choose 'add to namespace'.
### view responses
projectFolder> routes> web.php:
return view('...') ---> return $\frac{Inertia::render}{inertia}$('...')
>`use Inertia\Inertia;`
`Route::get('/', function () {`
`    return Inertia::render('');   //helper: return inertia('')`
`}`
# client-side
resources/js/app.js:
>return pages\[`./Pages/${name}.vue`]   //vue components: ./pages/componentName.vue

# vite
see: https://www.npmjs.com/package/@vitejs/plugin-vue
\>> npm i @vitejs/plugin-vue

./vite.config.js:
>see the link above
# run
\>> php artisan serve
\>> npm run dev

# Tailwind
./resources/js/app.js:
>`import '../css/app.css';`