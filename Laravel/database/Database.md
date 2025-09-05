# Localhost
browser>> https://localhost/phpmyadmin
# Factory
## Definition
با متد definition خروجی متداول و پیش فرض فکتوری مشخص می شود:
```php ln=false
public function definition(): array    {        
	return [            
		'name' => fake()->name(),            
		'email' => fake()->unique()->safeEmail(),            
		'email_verified_at' => now(),            
		'password' => static::$password ??= Hash::make('password'),            
		'remember_token' => Str::random(10),        
	];    
}
```
The `definition` method returns the default set of attribute values.
array $attributes: \[name,email, email_verified_at, password, remember_token]

## Factory States[:](https://laravel.com/docs/12.x/eloquent-factories#factory-states)
مواردیکه میخواهیم برخی اتریبیوت ها مقادیری غیر از حالت پیشفرض بگیرند باید متدهایی غیر از definition نیز بنویسیم و سپس این متدها را نیز در هنگام فراخوانی فکتوری با آن chain کنیم. مثلا تولید یوزری که معلق شده باشد:
```php ln=false
public function suspended(): Factory {    
	return $this->state(function (array $attributes) {        
		return [
			'account_status' => 'suspended',
		];
	});
/* or
	return $this->state(fn (array $attributes) => [            
		'email_verified_at' => null,        
	]);
*/
}
```

در هنگام فراخوانی مثلا در seeder:
```php ln=false
$user = User::factory()->suspended()->create();
$user = User::factory()->trashed()->create();
```
متد trashed یک متد درونی در لاراول است که اتریبیوت soft-delete را طوری تنظیم میکند که رکورد soft delete شده تولید شود.
# Resources
Every resource class defines a `toArray` method which returns the array of attributes that should be converted to JSON when the resource is returned as a response from a route or controller method.
- $this = model properties

