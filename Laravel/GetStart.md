
## config/database.php
'charset' => 'utf8mb4',
'collation' => 'utf8mb4_unicode_ci',
## Database Designing
معمولا کلید خارجی (مثل phone_id) را در والد نمیگذاریم (والد: users-table)
### create database
CREATE DATABASE $database_name CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
DROP DATABASE IF EXISTS $database_name;


# Artisans
در لاراول ترتیب منطقی ساخت اجزاء برای یک جدول یا موجودیت جدید به شکل زیر است:
Model → Migration → Factory → Seeder → Controller

اگر کامپوننتها/ویوها را قبل از کنترلر درست کنیم آنگاه کنترلر بر اساس ارتباط با کامپوننت/ویو درست خواهد شد.

```php ln=false title=Artisans
php artisan make:migration create_user_profiles_table
php artisan make:migration create_users_table
​	//اینها در دیتابیس جدول نمیسازند

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
## ساخت جدول در دیتابیس
```php ln=false title=
php artisan migrate:refresh --path=/database/migrations/2025_09_06_create_users_table.php
```
## [Seeders](https://laravel.com/docs/12.x/seeding#running-seeders)

```php ln=false title=
php artisan migrate:fresh --seed //rebuild tables
​	​	--class=UserSeeder   //only a specific seeder
php artisan db:seed
​	​	--class=UserSeeder   //only a specific seeder
```



 