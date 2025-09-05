# config/database.php
'charset' => 'utf8mb4',
'collation' => 'utf8mb4_unicode_ci',
# Tables Design
معمولا کلید خارجی (مثل phone_id) را در والد نمیگذاریم (والد: users-table)
# 2. Artisans Generate
در لاراول ترتیب منطقی ساخت اجزاء برای یک جدول یا موجودیت جدید به شکل زیر است:

| ترتیب |   جزئیات   | توضیح                                                                                |
|:-----:|:----------:| ------------------------------------------------------------------------------------ |
|   1   |   Model    | مدل به جدول متصل می‌شود و روابط و ویژگی‌ها را مشخص می‌کند.                           |
|   2   | Migration  | ابتدا جدول و ستون‌ها در دیتابیس ایجاد می‌شوند. بدون جدول، مدل و factory معنا ندارند. |
|   3   |  Factory   | پس از مدل و جدول، می‌توان داده‌های نمونه تولید کرد. Factory به مدل وابسته است.       |
|   4   |   Seeder   | و Seeder از Factory یا داده دستی برای پر کردن جدول استفاده می‌کند.                   |
|   5   | Controller | در نهایت، کنترلر برای مدیریت عملیات روی مدل‌ها (CRUD، API، View) نوشته می‌شود.       |
- ترتیب: Model → Migration → Factory → Seeder → Controller
```php ln=false
php artisan make:migration create_user_profiles_table

php artisan make:model UserProfile
	--factory | -f
	--seed | -s
	--controller | -c
	--controller --resource --requests | -crR
	-mfscrR //migration+factory+seed+controller+resource+request
	--all | -a //migration, factory, seeder, policy, controller, and form requests
	--pivot | -p //pivot model

php artisan make:factory UserProfileFactory

php artisan make:seeder UserProfileSeeder

php artisan make:controller UserProfileController
	--invokable -i //single method controller
	--resource  -r //with CRUD methods
	--requests  -R //generate request 

```
- بجز migration:snake_case همگی PascalCase هستند.
- بجز مدل در بقیه نام چیزی که میخواهیم بسازیم، پسوند می‌شود.
- پسوند و پیشوند در میگریشن: <span dir=rtl>create_&lt;name&gt;_table</span>
- فقط model پیشوند و پسوند ندارد.
- فقط migration پیشوند هم علاوه بر پسوند دارد.
- بجز مدل و کنترلر، در بقیه نیاز به پرچم flag نداریم.
- فقط در migration اسم را جمع می‌بندیم.
# ChatGPT Prompts
always:
in models cast datetime as Y/M/d-H:m
in models cast date as Y/M/d

 comment lines in codes as below for some less-known syntaxes or methods:
 return $this->belongsToMany(Role::class, 'role_users') 
            ->withPivot('assigned_by', 'assigned_at')   // include extra pivot columns
            ->withTimestamps();                         // update timestamps in pivot
 
 locate faker: $faker = \Faker\Factory::create('fa_IR');
 