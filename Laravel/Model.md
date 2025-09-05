```php ln=false
class Order extends Model
{
    use HasFactory;
    // -----------------------------
    // 1) Define the table name
    // -----------------------------
    protected $table = 'orders';
    // -----------------------------
    // 2) Mass assignable fields
    // -----------------------------
    protected $fillable = [
        'notification_date',
        'product_id',
        'quantity',
        'mount_of_order',
        'description',
        'status',
        'created_by',
    ];
    // -----------------------------
    // 3) Date fields casting
    // convert database getting to suitable Type: 'field_name' => 'type',
    // -----------------------------
	protected $casts = [
	    // بولین
	    'is_active'     => 'boolean',
	    'email_verified'=> 'bool',
	
	    // عددی
	    'age'           => 'integer',
	    'price'         => 'float',
	
	    // تاریخ
	    'created_at'    => 'datetime',
	    'birth_date'    => 'date',
	    'published_at'  => 'datetime:Y-m-d',
	
	    // آرایه / JSON
	    'settings'      => 'array',
	    'tags'          => 'json',
	    'meta'          => 'collection',
	
	    // رمزگذاری
	    'email'         => 'encrypted',
	    'api_token'     => 'encrypted',
	
	    // Enum
	    'status'        => OrderStatus::class,
	    'role'          => RoleEnum::class,
	    'address'       => AddressCast::class,
	];
    // -----------------------------
    // 4) Relationships
    // -----------------------------
    // An order belongs to a product
    public function product()
    {
        return $this->belongsTo(Product::class);
    }
    // An order may have many cutts
    public function cutts()
    {
        return $this->hasMany(Cutt::class, 'related_order_id');
    }
    // An order may have many deadlines
    public function deadlines()
    {
        return $this->hasMany(Deadline::class);
    }
}
```