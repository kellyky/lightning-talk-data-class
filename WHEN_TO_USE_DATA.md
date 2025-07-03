# Use Cases for Data class

Use Data class to create Data Objects when attribute values should not change, once defined.

## Example: Temperature Conversions

From a Temperature Conversion project, we use the Data class to define `Unit` and each 'unit' (e.g. Celsius) is an instance of `Unit`.

### Define Unit and UnitProxy as Data objects

```ruby
# `:proxy` is the conversion formula to the proxy unit
Unit = Data.define(:name, :proxy) do
  KELVIN_CELSIUS_OFFSET = 273.15r
end
```

Attributes, constants, methods defined here can be referenced by instances of `Unit`.

We can also pass an instance of `Unit` to another Data instance.
For example, we might want to create a Unit to serve as a proxy for conversions.

```ruby
# `:formulas` are conversion formulas to the target unit
UnitProxy = Data.define(:unit, :absolute_zero, :formulas)
```

### Create the units, proxy unit as instances of Unit, UnitProxy

For example, using `Kelvin` as the proxy and supporting conversions between `Celsius`, `Fahrenheit`, `Kelvin`, and `Newton`.

```ruby
Kelvin = Unit.new(
        name: :kelvin,
      kelvin: ->(temperature) { temperature },
)

Celsius = Unit.new(
        name: :celsius,
      kelvin: ->(temperature) { temperature + KELVIN_CELSIUS_OFFSET },
)

Fahrenheit = Unit.new(
        name: :fahrenheit,
      kelvin: ->(temperature) { 5/9r * (temperature - 32) + KELVIN_CELSIUS_OFFSET },
)

Newton = Unit.new(
        name: :newton,
      kelvin: ->(temperature) { temperature * 100/33r + KELVIN_CELSIUS_OFFSET },
)

# `Kelvin` is the instance of `Unit` Kelvin
KelvinInTheMiddle = UnitProxy.new(
  unit: Kelvin,
  absolute_zero: 0,
  formulas: {
        kelvin: ->(temperature) { temperature },
       celsius: ->(temperature) { temperature - KELVIN_CELSIUS_OFFSET },
    fahrenheit: ->(temperature) { (temperature - KELVIN_CELSIUS_OFFSET) * 9/5r + 32 },
        newton: ->(temperature) { (temperature - KELVIN_CELSIUS_OFFSET) * 33/100r },
  },
)
```

