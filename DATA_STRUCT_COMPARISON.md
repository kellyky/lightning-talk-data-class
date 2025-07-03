# Data class vs. Struct class

The main difference between instances of Data and Struct is mutability:
- Instances of Data are immutable
- Instances of Struct are mutable

## Use

### Data

```irb
>> House = Data.define(:rooms, :area, :floors)
>> ranch = House.new(rooms: 5, area: 1200, floors: 1)
=> #<data House rooms=5, area=1200, floors=1>

# Attempt to Renovate...

>> ranch.floors = 2
(irb):3:in `<main>': undefined method `floors=' for an instance of House (NoMethodError)
```

The attribute values (once created) are immutable.
Attempting to modify an attribute raises NoMethodError.

While we cannot update the `ranch` object to 2 floors,
we _can_ create a new object from `ranch` that does have 2 floors:

```irb
>> bungalow = ranch.with(floors: 2)
=> #<data House rooms=5, area=1200, floors=2>
```

### Struct

```irb
>> House = Struct.new(:rooms, :area, :floors, keyword_init: true)
>> ranch = House.new(rooms: 5, area: 1200, floors: 1)
=> #<struct House rooms=5, area=1200, floors=1>
>> ranch.floors = 2
=> 2
>> ranch
=> #<struct House rooms=5, area=1200, floors=2>
```

With Struct, we can renovate.

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

