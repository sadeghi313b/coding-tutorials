روابطی که در مدل تعریف می کنیم، در فایلهای js نیز قابل استفاده هستند. مثال: student.classroom.name
### 1. One To One
**توضیح**: یک مدل به یک مدل دیگر مرتبط است (مثلاً هر کاربر یک پروفایل دارد).
- **متدها**: `hasOne` (برای مدل والد)، `belongsTo` (برای مدل فرزند).
```php ln=false
// App/Models/User.php
public function profile()
{
    // Define a one-to-one relationship with Profile
    return $this->hasOne(Profile::class, $targetKey='user_id',$localKey='id');
    //this has one related - goto targetKey - equals localKey
}
public function related() {
​	return $this -> hasOne(Related::class, $keyhole, $key)
}

// App/Models/Profile.php
public function user()
{
    // Define the inverse one-to-one relationship with User
    return $this->belongsTo(User::class, $localKey='user_id', $targetKey='id');
    //this bolongs to related - by localKey - equals targetKey
}
public function related() {
​	return $this -> belongTo(Related::class, $key, $keyhole)
}
```
- key is in local table = some filed
- keyhole is in related(target) table = some field
- اگر فقط از سمت User به Profile نیاز دارید (مثل $user->profile)، نیازی به تعریف رابطه معکوس در Profile نیست.

- **استفاده**:
```php ln=false
$user = User::find(1);
$profile = $user->profile; // Get the user's profile
$user = Profile::find(1)->user; // Get the associated user
```

### 2. One To Many
**توضیح**: یک مدل به چندین مدل دیگر مرتبط است (مثلاً یک کاربر چندین پست دارد).
- **متدها**: `hasMany` (برای مدل والد)، `belongsTo` (برای مدل فرزند).
```php ln=false
// App/Models/User.php
public function posts()
{
    // Define a one-to-many relationship with Post
    return $this->hasMany(Post::class, 'user_id');
    //hasMany(ohterModel, foreign-key, loacal-key)
}

// App/Models/Post.php
public function user()
{
    // Define the inverse one-to-many relationship with User
    return $this->belongsTo(User::class, 'user_id');
    //belongsTo(ohterModel, foreign-key, owner-key)
}
```

- **استفاده**:
```php ln=false
$user = User::find(1);
$posts = $user->posts; // Get all posts by the user
$user = Post::find(1)->user; // Get the associated user
```

### 3. Many To Many
**توضیح**: چندین مدل به چندین مدل دیگر از طریق جدول واسطه(pivot-table) مرتبط هستند (مثلاً کاربران و نقش‌ها).
- **متد**: `belongsToMany` (برای هر دو مدل).
```php ln=false
// App/Models/User.php
// Define a many-to-many relationship with Role
class User extends Model → public function roles()
{
    return $this->belongsToMany(
    ​	Role::class, //joinTo: targetModel
    ​	$pivotTable='role_user' = alphabatic-order-pivotTableName,
    ​	$foreignPivotKey='user_id' //foreignPivotKey related to $thisModel
    ​	$foreignPivotKey='role_id', //foreignPivotKey related to targetModel
    ​	$primaryKey = 'id' //$thisModel
    ​	$primaryKey = 'id' //targetModel
    )
    //use1: foreach ($user->roles as $role) {}
    //use2: roles() method
    //   $roles = User::find(1)->roles()->orderBy('name')->get();
    //   $user->roles()->attach(1);
        
    ->using(RoleUser::class) //Custom many-to-many pivot Model: methods, casts etc.
    // app\model\RoleUser.php → class RoleUser extends Pivot {public $incrementing = true;}
    
    -> withPivot ('column1',,)
    // use: $user -> role -> pivot -> column1;
    -> as('aliasToPivot') // $user -> role -> aliasName -> column1;
    -> withTimeStamps () //$user-> role-> pivot -> created_at;
    
    ->wherePivot('approved', 1)
    ->wherePivotIn('priority', [1, 2])
    ->wherePivotNotIn('priority', [1, 2])
    ->wherePivotBetween('created_at', ['2020-01-01 00:00:00', '2020-12-31 00:00:00'])
    ->wherePivotNotBetween('created_at', ['2020-01-01 00:00:00', '2020-12-31 00:00:00'])
    ->wherePivotNull('expired_at')
    ->wherePivotNotNull('expired_at')
    
    ->orderByPivot('created_at', 'desc')
    ;  	
}

// App/Models/Role.php
// Define a many-to-many relationship with User
class Role extend Model → public function users()
{
    return $this->belongsToMany(User::class, ='role_user', ='user_id', ='id');
}
```
وقتی در لاراول یک رابطه‌ی `belongsToMany` تعریف می‌کنی، لاراول به صورت پیش‌فرض فقط ستون‌های کلید خارجی را از جدول میانی (**pivot table**) بارگذاری می‌کند. یعنی فقط role_id, user_id از جدول pivot. برای گرفتن بقیه‌ی ستون‌های جدول میانی (`assigned_by`, `assigned_at`) باید از متد withPivot استفاده کرد.

نکته مهم: اگر یک مدل مستقل برای RoleUser درست کنیم آنگاه لاراول طبق قواعدش همیشه با صدا زدن این مدل، جدول role_users را برایش در نظر می‌گیرد درحالیکه جدول ما role_user به شکل مفرد است. برای حل این مشکل باید در مدل RoleUser خط زیر را درون کلاس RoleUser Class برای تصریح نام جدول خارج از قواعد داشته باشیم:
protected $table = 'role_user';
### 4. Has One Through
**توضیح**: یک مدل به یک مدل دیگر از طریق یک مدل واسطه مرتبط است (مثلاً یک کاربر یک آدرس دارد از طریق پروفایل).
- **متد**: `hasOneThrough`.
```php ln=false
// App/Models/User.php
public function address()
{
    // Define a has-one-through relationship with Address via Profile
    return $this -> hasOneThrough (Address::class, Profile::class, 'user_id', 'profile_id', 'id', 'id');
    //hasOneThrough(final-model, intermediate-model
	//               intermediate-Fkey, final-Fkey, 
	//               local-Pkey, intermediate-Pkey)
}
```

- **استفاده**:
```php ln=false
$user = User::find(1);
$address = $user->address; // Get the user's address through their profile
```

### 5. Has Many Through
**توضیح**: یک مدل به چندین مدل دیگر از طریق یک مدل واسطه مرتبط است (مثلاً یک کشور چندین پست دارد از طریق کاربران).
- **متد**: `hasManyThrough`.
```php ln=false
// App/Models/Country.php
public function posts()
{
    // Define a has-many-through relationship with Post via User
    return $this -> hasManyThrough(Post::class, User::class, 'country_id', 'user_id', 'id', 'id');
}
```

- **استفاده**:
```php ln=false
$country = Country::find(1);
$posts = $country->posts; // Get all posts by users in the country
```

### 6. One To One (Polymorphic)
**توضیح**: یک مدل به یک مدل دیگر از نوع‌های مختلف مرتبط است (مثلاً یک تصویر به یک کاربر یا پست تعلق دارد).
- **متدها**: `morphOne` (برای مدل والد)، `morphTo` (برای مدل فرزند).
```php ln=false
// App/Models/Image.php
public function imageable()
{
    // Define the polymorphic relationship
    return $this->morphTo();
}

// App/Models/User.php
public function image()
{
    // Define a one-to-one polymorphic relationship with Image
    return $this->morphOne(Image::class, 'imageable');
}

// App/Models/Post.php
public function image()
{
    // Define a one-to-one polymorphic relationship with Image
    return $this->morphOne(Image::class, 'imageable');
}
```

- **استفاده**:
```php ln=false
$user = User::find(1);
$image = $user->image; // Get the user's image
$imageable = Image::find(1)->imageable; // Get the associated model (User or Post)
```

### 7. One To Many (Polymorphic)
**توضیح**: یک مدل به چندین مدل دیگر از نوع‌های مختلف مرتبط است (مثلاً نظرات به کاربران و پست‌ها تعلق دارند).
- **متدها**: `morphMany` (برای مدل والد)، `morphTo` (برای مدل فرزند).
```php ln=false
// App/Models/Comment.php
public function commentable()
{
    // Define the polymorphic relationship
    return $this->morphTo();
}

// App/Models/User.php
public function comments()
{
    // Define a one-to-many polymorphic relationship with Comment
    return $this->morphMany(Comment::class, 'commentable');
}

// App/Models/Post.php
public function comments()
{
    // Define a one-to-many polymorphic relationship with Comment
    return $this->morphMany(Comment::class, 'commentable');
}
```

- **استفاده**:
```php ln=false
$post = Post::find(1);
$comments = $post->comments; // Get all comments for the post
$commentable = Comment::find(1)->commentable; // Get the associated model (User or Post)
```

### 8. Many To Many (Polymorphic)
**توضیح**: چندین مدل به چندین مدل دیگر از نوع‌های مختلف از طریق جدول واسطه مرتبط هستند (مثلاً تگ‌ها به پست‌ها و ویدئوها).
- **متدها**: `morphToMany` و `morphedByMany`.
```php ln=false
// App/Models/Tag.php
public function posts()
{
    // Define a many-to-many polymorphic relationship with Post
    return $this->morphedByMany(Post::class, 'taggable');
}

// App/Models/Post.php
public function tags()
{
    // Define a many-to-many polymorphic relationship with Tag
    return $this->morphToMany(Tag::class, 'taggable');
}

// App/Models/Video.php
public function tags()
{
    // Define a many-to-many polymorphic relationship with Tag
    return $this->morphToMany(Tag::class, 'taggable');
}
```

- **استفاده**:
```php ln=false
$post = Post::find(1);
$tags = $post->tags; // Get all tags for the post
$post->tags()->attach(1); // Attach tag with ID 1 to the post
```

## نکات کلیدی

- **کلیدهای خارجی**: در روابط One To One و One To Many، کلید خارجی (`foreign_key`) در جدول فرزند قرار دارد (مثل `user_id`).
- **روابط پلی‌مورفیک**: نیاز به ستون‌های `_id` و `_type` در جدول فرزند یا واسطه دارند (مثل `imageable_id` و `imageable_type`).
- **جدول واسطه**: در Many To Many و Many To Many (Polymorphic) برای ذخیره روابط استفاده می‌شود.
- **مدیریت روابط**: از `attach` و `detach` برای مدیریت روابط Many To Many و Many To Many (Polymorphic) استفاده کنید.