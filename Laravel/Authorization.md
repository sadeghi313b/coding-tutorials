# Gate 
```php ln=false title=AuthServiceProvider.php
public function boot(): void{  
	// gates with bool response  
	Gate::define('update-post', //abilityName
		//callback function
		function (User $user, Post $post) { // arg1:userInstance, arg2:aModel       
			return $user->id === $post->user_id; // return true|false ≡ bool 
		}
		___or___
		// callback class
		Gate::define('update-post', [PostPolicy::class, 'update']); 
	); ≡bool
	


}
```

```php ln=false title=controller
//closure-arg1=current user :auto injectection
Gate::allows|check ('ability-name', $model§-toPass-toArg2orMore ); ≡bool 

//closure-arg1=$someUser :manual injectection
Gate::forUser($someUser) ->
	allows|denies ('ability-name',$model-toPass_toArg2); ≡bool
	any|none      ('ability-name',$models-toPass_toArg2andMore); ≡bool

if (! Gate::allows('update-post', $post)) { abort(403); }

Gate::authorize ('ability-name', $model); 
	≡continue-if-gateTrue
	|≡stop+throwing-exception-if-gateFalse

```

```php ln=false title=
◙ AuthServiceProvider.php:
	// define with nonBool response: set authorization response
	Gate::define('edit-settings', function (User $user) {    
		return $user->isAdmin 
			? Response::allow() 
			: Response::deny('You must be an administrator.');
			__or__
			: Response::denyWithStatus(404) _or_ ::denyAsNotFound();
	});

◙ controller.php: 
	// return bool
	Gate::allows ~~ ≡bool
	
	// return response
	$response = Gate::inspect('edit-settings');
	if ($response->allowed()) 
		{ echo'The action is authorized' } 
	else 
		{echo $response->message();} 
	
	// return error message
	Gate::authorize('edit-settings'); 
		≡continue-if-responeAllowed
		|≡stop+error message will be propagated to the HTTP response
```

وقتی بخواهی یک نقش خاص (مثلاً admin) همه دسترسی‌ها را داشته باشد و نیاز نباشد در همه Gateها جداگانه شرط `if ($user->isAdministrator())` بنویسیم: اگر Gate::befor مقدار درستی را برگرداند آنگاه این مقدار بازای فراخوانی هر گیتی جایگزین می‌شود. اگر null برگرداند آنگاه گیت اصلی بررسی خواهد شد. در واقع یک نوع befor-byPass برای همۀ ability هاست.
اینجا Gate::befor درونش یک فانکشن دارد که با فراخوانی یک گیت، $user و $ability به این فانکشن پاس داده می‌شود. اگر نتیجه true بود آنگاه جایگزین می‌شود واگرنه گیت اصلی بررسی خواهد شد.
```php ln=false title=
◙ AuthServiceProvider.php: out of boot method

Gate::before(function (User $user, string $ability) {    
	if ($user->isAdministrator()) { return true; }
});

Gate::after(function (User $user, string $ability, bool|null $result, mixed $arguments) {    
	if ($user->isAdministrator()) { return true; }
});
```

دروازۀ befor قبل از اجرای Gateهای تعریف‌شده صدا زده می‌شود. می‌تواند **اجازه یا رد** را سریع برگرداند. اما after  بعد از اجرای Gate اصلی صدا زده می‌شود. نتیجه (`true` یا `false`) بهش پاس داده می‌شود و تو می‌توانی آن را تغییر بدهی. 
```php ln=false title=
use Illuminate\Support\Facades\Gate;
use Illuminate\Support\Facades\Log;

Gate::after(function ($user, string $ability, ?bool $result, $arguments) {
    // ذخیره لاگ برای هر بررسی دسترسی
    Log::info('Gate check', [
        'user_id'  => $user->id,
        'ability'  => $ability,
        'result'   => $result ? 'allowed' : 'denied',
        'arguments'=> $arguments, // مثلا $post
    ]);

    // اگر ادمین باشد، دسترسی را همیشه آزاد کن
    if ($user->isAdministrator()) {
        return true;
    }
});
```
لاراول اول Gate اصلی `delete-post` را بررسی می‌کند. سپس کلوزر فانکشن درون after صدا زده می‌شود. اینگونه در جدول لاگ ثبت می‌شود که: 
- کدام کاربر
- روی چه ability
- با چه نتیجه‌ای (allowed/denied)
- روی چه مدلی (مثلاً `$post`)
- گر کاربر **ادمین** باشد → نتیجه نهایی همیشه `true` می‌شود، حتی اگر Gate اصلی گفته باشد `false`.
این روش برای **نظارت** روی همه دسترسی‌ها خیلی عالی است. مثلاً می‌توانی ببینی چه کسی سعی کرده چیزی را حذف کند ولی اجازه نداشته.
### [Inline Authorization](https://laravel.com/docs/12.x/authorization#inline-authorization)
در حالت عادی، شما باید اول در `AuthServiceProvider` گیت تعریف کنی و بعد در کنترلر فراخوانی‌اش کنی. اما در **Inline Authorization** دیگر نیازی به تعریف Gate قبلی نیست.
```php ln=false title=controller
// اجازه بده فقط اگر کاربر ادمین باشد
Gate::allowIf(fn(User $user) => $user->isAdministrator()); 
	≡stop+throw-exception | continue

// ممنوع کن اگر کاربر بن شده باشد
Gate::denyIf(fn(User $user) => $user->banned());
	≡stop+throw-exception | continue
```
البته Inline Authorization **هیچ‌کدام از hookهای before/after** را اجرا نمی‌کند. یعنی اگر `Gate::before` یا `Gate::after` تعریف کرده باشی، روی این دستورات اعمال نمی‌شوند.
<hr style="height:6px; border:none; background:blue ; margin:16px 0;">
# [Policies](https://laravel.com/docs/12.x/authorization#creating-policies)
Policies are classes that organize authorization logic, around a particular model or resource.
> **php artisan make:policy PostPolicy**   `//model: Post`
> 		--model=Post `//methods related to viewAny, view, create, update, delete, restore, and forceDelete`

```php ln=false title=PostPolicy.php→class:PostPolicy
class PostPolicy {
    public function update(User $user, Post $post): bool { 
	    return $user->id === $post->user_id;    
	}
	
	// Methods Without Models
	public function create(User $user): bool{
		return $user->role == 'writer';
	}
	
	public function update(User $user, Post $post): Response {
		return $user->id === $post->user_id 
			? Response::allow() 
			: Response::deny('You do not own this post.'); _OR_ : Response::denyWithStatus(404);
	}
}
```

```php ln=false title=controller
Gate::authorize('update', $post); ≡continue_ifTrue|Exception-ifFalse

$response = Gate::inspect('update', $post);
~ $response->allowed() ≡bool
```

## Policy Filters
```php ln=false title=PostPolicy.php→class:PostPolicy

public function before(User $user, string $ability): bool|null { 
	if ( $user->isAdministrator() ) { return true; }     
	return null;
}

```
### رفتار:
- اگر `true` ← همه abilities (update, delete, view, …) **قبول می‌شوند**.
- اگر `false` ← همه abilities **رد می‌شوند**.
- اگر `null` ← لاراول بررسی را می‌سپارد به همان متد policy (مثلاً `update`).
## [Using Policies](https://laravel.com/docs/12.x/authorization#authorizing-actions-using-policies)
### [Via the User Model](https://laravel.com/docs/12.x/authorization#via-the-user-model)
The `App\Models\User` model that is included with your Laravel application includes two helpful methods for authorizing actions: `can` and `cannot`. These methods receive the name of the action you wish to authorize and the relevant model.
```php ln=false title=PostController.php
	public function update(Request $request, Post $post): RedirectResponse { 
		if ( $request->user()->cannot('update', $post) ) { 
			abort(403);        
		}         
		// Update the post...         
		return redirect('/posts');    
	}
```

- Actions That Don't Require Models
In these situations, you may pass a class name to the `can` method. The class name will be used to determine which policy to use when authorizing the action
```php ln=false title=PostController.php	
	// Actions That Don't Require Models
	public function store(Request $request): RedirectResponse    {        
		if ( $request->user()->cannot('create', Post::class) ) { abort(403); }        
		// Create the post...         
		return redirect('/posts');    
	}

```
### [Via the Gate Facade](https://laravel.com/docs/12.x/authorization#via-the-gate-facade)
authorize actions via the `Gate` facade's `authorize` method. Like the `can` method, this method accepts the name of the action you wish to authorize and the relevant model.
```php ln=false title=PostController.php	
	public function update(Request $request, Post $post): RedirectResponse {
		Gate::authorize('update', $post); continue|exception:HTTP-response-with403         
		// The current user can update the blog post...         
		return redirect('/posts');    
	}
	
	//// Actions That Don't Require Models
	public function create(Request $request): RedirectResponse{    
		Gate::authorize('create', Post::class);     
		// The current user can create blog posts...     
		return redirect('/posts');
	}
```
### [Via Middleware](https://laravel.com/docs/12.x/authorization#via-middleware)
```php ln=false title=
Route::put('/post/{post}', function (Post $post) {    
	// The current user may update the post...
})->middleware('can:update,post') _or_ ->can('update', 'post');
```

Actions that don't require models:
```php ln=false title=
Route::post('/post', function () {    
	// The current user may create posts...
})->middleware('can:create,App\Models\Post') _OR_ ->can('create', Post::class);
```

### [Via Blade Templates](https://laravel.com/docs/12.x/authorization#via-blade-templates)
```php ln=false title=
@can('update', $post)  //≡≡@if (Auth::user()->can('update', $post))
    <!-- The current user can update the post... -->
@elsecan('create', App\Models\Post::class)
    <!-- The current user can create new posts... -->
@else
    <!-- ... -->
@endcan

@cannot('update', $post)
    <!-- The current user cannot update the post... -->
@elsecannot('create', App\Models\Post::class)
    <!-- The current user cannot create new posts... -->
@endcannot
```

if a user is authorized to perform any action from a given array of actions:
```php ln=false title=
@canany(['update', 'view', 'delete'], $post)
    <!-- The current user can update, view, or delete the post... -->
@elsecanany(['create'], \App\Models\Post::class)
    <!-- The current user can create a post... -->
@endcanany
```

Actions that don't require models:
```php ln=false title=
@can('create', App\Models\Post::class)
    <!-- The current user can create posts... -->
@endcan

@cannot('create', App\Models\Post::class)
    <!-- The current user can't create posts... -->
@endcannot
```
## [Supplying Additional Context](https://laravel.com/docs/12.x/authorization#supplying-additional-context)
```php ln=false title=PostPolicy.php
public function update(User $user, Post $post, int $category): bool
{
    return $user->id === $post->user_id &&
           $user->canUpdateCategory($category);
}
```

```php ln=false title=PostController.php
public function update(Request $request, Post $post): RedirectResponse
{
    Gate::authorize('update', [$post, $request->category]);

    // The current user can update the blog post...

    return redirect('/posts');
}
```
## [Authorization & Inertia](https://laravel.com/docs/12.x/authorization#authorization-and-inertia)
چک کردن مجوزها همیشه **سمت سرور** انجام می‌شود (امنیت). اما برای نمایش درست **UI** (مثل مخفی کردن یا نمایش دکمه‌ها) نیاز داریم وضعیت مجوزها را سمت فرانت هم داشته باشیم. به همین دلیل این اطلاعات را از سرور در `HandleInertiaRequests` به همه‌ی صفحات می‌دهیم.
```php ln=false title=HandleInertiaRequests.php
class HandleInertiaRequests extends Middleware
{
    public function share(Request $request)
    {
        return [
            ...parent::share($request),

            // اضافه کردن داده‌های احراز هویت و دسترسی
            'auth' => [
                'user' => $request->user(),  // خود کاربر لاگین‌شده

                'permissions' => [
                    'post' => [
                        // آیا کاربر می‌تواند پست بسازد؟
                        'create' => $request->user()->can('create', Post::class),
                    ],
                ],
            ],
        ];
    }
}

```
- **سمت بک‌اند**:
    - در متد share هر چیزی که return شود به **تمام صفحات Inertia** پاس داده می‌شود.
    - اینجا auth.user و auth.permissions به همه ویوها می‌رسد.
- **سمت فرانت‌اند (Vue/React)**:
    - شما می‌توانید در هر صفحه به راحتی از page.props.auth.user یا page.props.auth.permissions.post.create استفاده کنید.
    - مثلاً دکمه "ایجاد پست جدید" فقط وقتی رندر شود که مقدار permissions.post.create === true باشد.
