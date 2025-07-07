## Immutability Workarounds, Sort of

We *can* work (arguably) against the intent of `Data` in some ways.

For example, when an attribute is set to a `Hash`, we can update values in the hash.

```ruby
ColorCounts = Data.define(:color_tally)
# => Colorcounts

screen_colors = ColorCounts.new({ red: 0, blue: 0, green: 0})
# => #<data ColorCounts color_tally={:red=>0, :blue=>0, :green=>0}>

# Update a value in `color_tally`
screen_colors.color_tally[:red] += 5
# => 5

# Note that the value for  `:red` has been updated
screen_colors.color_tally
# => {:red=>5, :blue=>0, :green=>0}
```

We can also workaround to update a (non-frozen) string.

Example:

```ruby
Person = Data.define(:name)
# => Person

me = Person.new(name: 'Kelly')
# => #<data Person name="Popko">

me.name.prepend 'Kelly '
# => 'Kelly Popko'

me.name
# => 'Kelly Popko'
```

So we are not completely without the ability to modify or create certain workarounds.

However, we would want to choose something else more suited to what we need instead of trying to work around it with `Data`.

### Communication Considerations

What do we want to suggest about the object to future contributors?

Use of `Data` communicates to future collaborators to think of the object as more like a value object - `nil`, `true`, `false`, etc.

Use of `Class`, `Struct`, `Openstruct`, for example, communicate more flexibility (in different ways).

