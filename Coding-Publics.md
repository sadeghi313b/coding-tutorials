# Enum
Laravel
```php ln=false title=
<?php
namespace App\Enums;
enum OrderStatus: string
{
    case Active   = 'active';
    case Force    = 'force';
    case Hold     = 'hold';
    case Canceled = 'canceled';
    case Enough   = 'enough';
    // optional: helper for showing labels
    public function label(): string
    {
        return match ($this) {
            self::Active   => 'Active',
            self::Force    => 'Force',
            self::Hold     => 'On Hold',
            self::Canceled => 'Canceled',
            self::Enough   => 'Enough',
            default => 'Error OrderStatus Enum',
        };
    }
    // optional: get all values
    public static function values(): array
    {
        return array_column(self::cases(), 'value');
    }
    // optional: get as array for dropdowns
    public static function options(): array
    {
        return array_map(fn($case) => [
            'label' => $case->label(),
            'value' => $case->value,
        ], self::cases());
    }
}
```


```php ln=false title=
//Model
protected $casts = [ 'status' => OrderStatus::class, ];

//Request@rules
return [ 'status' => ['required', new Enum(OrderStatus::class)], ];

//send to front
OrderStatus::options()
[
  {"label": "Active", "value": "active"},
  {"label": "Force", "value": "force"},
  {"label": "On Hold", "value": "hold"},
  {"label": "Canceled", "value": "canceled"},
  {"label": "Enough", "value": "enough"}
]

//usage
$status = OrderStatus::Active; echo $status->label();
```
