در PHP وقتی یک کلاس را **شیءسازی (instantiate)** می‌کنیم، هر نمونه (object) ساخته‌شده یک فضای مخصوص به خودش دارد. کلمه کلیدی `$this` درون کلاس، **به همان نمونه‌ای اشاره می‌کند که متد یا ویژگی در حال اجرا روی آن فراخوانی شده است**. یعنی `$this` همیشه نشان‌دهنده‌ی **"این شیء جاری"** است.
```php ln=false
class Car {
    public $color;

    public function setColor($color) {
        $this->color = $color; // اشاره به property همین object
    }

    public function getColor() {
        return $this->color;
    }
}
```

وقتی یک کلاس از دیگری ارث می‌برد، <span dir=ltr>$this</span> همچنان به شیء نهایی (Final Object) اشاره می‌کند، نه صرفاً کلاس والد**. بنابراین اگر متد حاوی <span dir=ltr>$this</span> در والد تعریف شده باشد، با ارث بری این متد در فراخوانی کلاس فرزند نیز قابل استفاده است. حالا وقتی در کلاس فرزند فراخوانی می شود آنگاه همان کلاس فرزند را برمیگرداند.
# Traits - Use
تریت درواقع یک جور **کد قابل اشتراک‌گذاری** بین چند کلاس با use است. بنابراین trait خودش object ندارد، فقط کد را وارد کلاس می‌کند. وقتی درون یک trait از `$this` استفاده می‌کنیم، `$this` به شیء **کلاسی اشاره می‌کند که trait در آن use شده است**. بنابراین `$this` درون `trait` به **شیء کلاس مصرف‌کننده‌ی trait** اشاره می‌کند.
```php ln=false
trait Logger {
    public function log($msg) {
        echo "[" . get_class($this) . "] $msg\n";
    }
}

class User {
    use Logger;
}

class Product {
    use Logger;
}

$user = new User();
$user->log("User created"); 
// [User] User created

$product = new Product();
$product->log("Product added"); 
// [Product] Product added
```
# Interface - Implements
```php ln=false
interface Animal {
	//هر چیزی که حیوان است، باید بتواند صدا تولید کند و حرکت کند
    public function makeSound();
    public function move();
}

class Dog implements Animal {
    public function makeSound() {
        echo "Woof! Woof!";
    }
    public function move() {
        echo "Running on four legs.";
    }
    //اگر هر کدام از این متدها را فراموش کنی، خطا می‌دهد
}
```