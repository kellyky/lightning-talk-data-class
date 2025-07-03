## Description

From the Ruby Language Data class official documentation,
> defines simple classes for value-alike objects
> is meant to be a storage for immutable atomic values
\- Source: [Ruby 3.2 Data documentation](https://docs.ruby-lang.org/en/3.2/Data.html)

## Syntax

### Create the blueprint (instance of Data class)

We call `Data.define` and pass keyword arguments.

```ruby
Book = Data.define(:title, :author, :year)
```

We can define methods as well if we pass it a block.

```ruby
Book = Data.define(:title, :author, :year) do
  SUMMARY = '%<title>s was written by %<author>s in %<year>i'

  def to_s
    SUMMARY % {title:, author:, year:}
  end
end
```

```ruby
hobbit = Book.new("The Hobbit", "J.R.R. Tolkien", 1937)
puts hobbit.to_s
=> 'The Hobbit was written by J.R.R. Tolkien in 1937'
```

### Creating the Data objects

Example:

```ruby
hobbit = Book.new(title: "The Hobbit", author: "J. R. R. Tolkien", year: 1937)
=> <data Book title="The Hobbit", author="J. R. R. Tolkien", year=1937>

hobbit.author
=> "J. R. R. Tolkien"

hobbit.year
=> 1937

hobbit.title
=> "The Hobbit"
```

We can create shallow clones of an instance of Data
Example:

```ruby

>> philadelphia = Location[39.9526, -75.1652]
=> #<data Location latitude=39.9526, longitude=-75.1652>
>> philadelphia.latitude
=> 39.9526
>> philadelphia.longitude
=> -75.1652
```

- Initialize with keyword arguments
- Initialize with positional arguments
- Initialize with brackets (either with keyword or positional arguments)

