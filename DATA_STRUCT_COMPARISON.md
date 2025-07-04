# Data class vs. Struct class

In practice, a main difference between instances of Data and Struct is mutability:
- Instances of Data are immutable
- Instances of Struct are mutable

Data also has fewer (and different) 'built-in' methods than Struct.

## Create New Instance

### Data

```irb
>> House = Data.define(:rooms, :area, :floors)
>> ranch = House.new(rooms: 5, area: 1200, floors: 1)
=> #<data House rooms=5, area=1200, floors=1>
```

### Struct

```irb
>> House = Struct.new(:rooms, :area, :floors, keyword_init: true)
>> ranch = House.new(rooms: 5, area: 1200, floors: 1)
=> #<struct House rooms=5, area=1200, floors=1>
```

## Mutability: Attempt to Renovate the house...

### Data

```irb
>> House = Data.define(:rooms, :area, :floors)
>> ranch = House.new(rooms: 5, area: 1200, floors: 1)
=> #<data House rooms=5, area=1200, floors=1>
>> ranch.floors = 2
(irb):3:in `<main>': undefined method `floors=' for an instance of House (NoMethodError)
```

This tells us there is no 'setter' method to use.

If we try to define a setter method on `House` and update that value, `FrozenError` is raised.

```irb
>> House = Data.define(:rooms, :area, :floors) do
>>   def floors=(quantity)
>>     @floors = quantity
>>   end
>> end
=> House
>> cottage = House.new(rooms: 5, area: 700, floors: 1)
=> #<data House rooms=5, area=700, floors=1>
>> cottage.floors = 2
(irb):21:in `floors=': can't modify frozen House: #<data House rooms=5, area=700, floors=1> (FrozenError)
```

But what we _can_ do with a Data object is create a copy of it and update any of the attribute values.

```irb
>> House = Data.define(:rooms, :area, :floors)
>> cottage = House.new(rooms: 5, area: 1200, floors: 1)
=> #<data House rooms=5, area=1200, floors=1>
>> two_story_cottage = cottage.with(floors: 2)
=> #<data House rooms=5, area=1200, floors=2>
```

### Struct

With Struct, we can renovate.

```irb
>> House = Struct.new(:rooms, :area, :floors, keyword_init: true)
>> ranch = House.new(rooms: 5, area: 1200, floors: 1)
=> #<struct House rooms=5, area=1200, floors=1>
>> ranch.floors = 2
=> 2
>> ranch
=> #<struct House rooms=5, area=1200, floors=2>
```

## Built-in Methods

### Data Class

```irb
>> Data.methods(false)
=> [:define]
>> Data.instance_methods(false)
=>
[:deconstruct_keys,
 :pretty_print,
 :pretty_print_cycle,
 :deconstruct,
 :hash,
 :==,
 :inspect,
 :members,
 :to_s,
 :with,
 :eql?,
 :to_h]
```

### Struct

```irb
>> Struct.methods(false)
=> [:new]
>> Struct.instance_methods(false)
=>
[:deconstruct_keys,
 :==,
 :members,
 :to_a,
 :to_s,
 :[],
 :[]=,
 :values_at,
 :eql?,
 :to_h,
 :select,
 :filter,
 :pretty_print,
 :pretty_print_cycle,
 :deconstruct,
 :hash,
 :inspect,
 :each_pair,
 :values,
 :length,
 :dig,
 :size,
 :each]
```

### Comparison

```irb
>> struct_methods - data_methods
=>
[:to_a,
 :[],
 :[]=,
 :values_at,
 :select,
 :filter,
 :each_pair,
 :values,
 :length,
 :dig,
 :size,
 :each]

>> data_methods - struct_methods
=> [:with]
```

