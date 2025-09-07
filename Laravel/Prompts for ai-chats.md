# Chat with ai Prompts
always:
in models cast datetime as Y/M/d-H:m
in models cast date as Y/M/d

 comment lines in codes as below for some less-known syntaxes or methods:
 return $this->belongsToMany(Role::class, 'role_users') 
            ->withPivot('assigned_by', 'assigned_at')   // include extra pivot columns
            ->withTimestamps();                         // update timestamps in pivot
 
 locate faker: $faker = \Faker\Factory::create('fa_IR');