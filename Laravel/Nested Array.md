To extract all `numbers` from a nested array structure where you have `phone[*].numbers`, here are several approaches:

## 1. **Using `array_column()`** (Simplest)
```php
$data = [
    'phone' => [
        ['numbers' => '123-456-7890', 'type' => 'home'],
        ['numbers' => '098-765-4321', 'type' => 'work'],
        ['numbers' => '555-123-4567', 'type' => 'mobile']
    ]
];

$numbers = array_column($data['phone'], 'numbers');
// Result: ['123-456-7890', '098-765-4321', '555-123-4567']
```

## 2. **Using Collection `pluck()`** (Laravel)
```php
use Illuminate\Support\Arr;

$data = [
    'phone' => [
        ['numbers' => '123-456-7890', 'type' => 'home'],
        ['numbers' => '098-765-4321', 'type' => 'work'],
        ['numbers' => '555-123-4567', 'type' => 'mobile']
    ]
];

$numbers = Arr::pluck($data['phone'], 'numbers');
// Or with Collection
$numbers = collect($data['phone'])->pluck('numbers')->all();
```

## 3. **Using `array_map()`**
```php
$data = [
    'phone' => [
        ['numbers' => '123-456-7890', 'type' => 'home'],
        ['numbers' => '098-765-4321', 'type' => 'work'],
        ['numbers' => '555-123-4567', 'type' => 'mobile']
    ]
];

$numbers = array_map(function($item) {
    return $item['numbers'];
}, $data['phone']);
```

## 4. **For Complex Nested Structures**
```php
$data = [
    'users' => [
        [
            'name' => 'John',
            'phone' => [
                ['numbers' => '111-222-3333', 'type' => 'home'],
                ['numbers' => '444-555-6666', 'type' => 'work']
            ]
        ],
        [
            'name' => 'Jane',
            'phone' => [
                ['numbers' => '777-888-9999', 'type' => 'mobile']
            ]
        ]
    ]
];

// Extract all numbers from all users
$allNumbers = [];
foreach ($data['users'] as $user) {
    $allNumbers = array_merge($allNumbers, array_column($user['phone'], 'numbers'));
}
// Result: ['111-222-3333', '444-555-6666', '777-888-9999']
```

## 5. **Using Collection Fluent Style** (Laravel - Recommended)
```php
use Illuminate\Support\Arr;

$data = [
    'phone' => [
        ['numbers' => '123-456-7890', 'type' => 'home'],
        ['numbers' => '098-765-4321', 'type' => 'work'],
        ['numbers' => '555-123-4567', 'type' => 'mobile']
    ]
];

$numbers = collect($data['phone'])->pluck('numbers')->toArray();
```

## 6. **With Error Handling**
```php
function extractNumbers($data) {
    if (!isset($data['phone']) || !is_array($data['phone'])) {
        return [];
    }
    
    return array_column($data['phone'], 'numbers');
}

$numbers = extractNumbers($data);
```

## 7. **For Multiple Levels of Nesting**
```php
$data = [
    'contacts' => [
        'phone' => [
            ['numbers' => '123-456-7890', 'type' => 'home'],
            ['numbers' => '098-765-4321', 'type' => 'work']
        ]
    ]
];

// Using null coalescing and array_column
$numbers = array_column($data['contacts']['phone'] ?? [], 'numbers');
// Result: ['123-456-7890', '098-765-4321']
```

## 8. **Real-World API Response Example**
```php
public function getUserPhones($userId)
{
    $user = User::with('phones')->findOrFail($userId);
    
    // Extract just the phone numbers array
    $phoneNumbers = array_column($user->phones->toArray(), 'numbers');
    
    return response()->json([
        'user_id' => $userId,
        'phone_numbers' => $phoneNumbers,
        'count' => count($phoneNumbers)
    ]);
}
```

## 9. **Using array_reduce()** (Alternative approach)
```php
$data = [
    'phone' => [
        ['numbers' => '123-456-7890', 'type' => 'home'],
        ['numbers' => '098-765-4321', 'type' => 'work']
    ]
];

$numbers = array_reduce($data['phone'], function($carry, $item) {
    $carry[] = $item['numbers'];
    return $carry;
}, []);
```

## 🚀 **Most Efficient Solution:**
```php
// For simple extraction
$numbers = array_column($data['phone'], 'numbers');

// For Laravel applications
$numbers = Arr::pluck($data['phone'], 'numbers');
// or
$numbers = collect($data['phone'])->pluck('numbers')->toArray();
```

## 📋 **Sample Output:**
**Input:**
```php
$data = [
    'phone' => [
        ['numbers' => '+1-123-456-7890', 'type' => 'home'],
        ['numbers' => '+1-987-654-3210', 'type' => 'work'],
        ['numbers' => '+1-555-123-4567', 'type' => 'mobile']
    ]
];
```

**Output:**
```php
[
    '+1-123-456-7890',
    '+1-987-654-3210', 
    '+1-555-123-4567'
]
```

## ✅ **Recommendation:**
Use **`array_column($data['phone'], 'numbers')`** for simple cases or **Laravel Collections** for more complex data manipulation:

```php
// Simple PHP
$numbers = array_column($data['phone'] ?? [], 'numbers');

// Laravel
$numbers = Arr::pluck($data['phone'] ?? [], 'numbers');
```

This will extract all `numbers` values from the `phone` array into a clean, flat array.