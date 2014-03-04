# The Objective Ruby Style-guide

** This is a stub for the real style-guide, send pulls, argue with me etc. **

The Objective Ruby Style-guide aims to provide guidance on Ruby coding style that is
based on objective fact and real benefits over more subjective reasons such as
"I've always done it that way" or "I prefer the way that looks".

## Goals

### Consistency

If there are two alternatives for an expression, like quotes for example, pick
one for *an objective reason* and use that one *all the time*.

### Reduce churn

Style that results in code that can be added to modified by simply adding lines
rather than changing existing ones will be preferred.
Like the open/closed principle but for diffs.

### Reduce use of keywords

In Ruby everything is an object and almost everything is a method.
This gives great power to its users. This style-guide will favour use of Ruby's
methods over its more mysterious keywords.

## Quoted strings

Always use double quotes.

## More complex strings

Always use `%{"a string that is maybe a quote"}`

## Blocks

Always use `{ }`, never use 'do end'

* Text editors are *MUCH* better at understanding braces
* Chaining and functional operations are clearer
* Imperative operations like `#each` do not suffer

## Collections

### Spread collections across multiple lines
Always define collections across multiple lines, giving each element its own
line. Elements must not share lines with their containing tokens `[{(`.

* Always add a trailing comma to the last element *

```ruby
  # Objectively inferior
  [ "one", "two", "three" ]

  # Objectively superior
  [
    "one",
    "two",
    "three",
  ]
```

## Hashes

Never use the `#[]` accessor method, Always use `#fetch` and `#store`

## Methods

### Def keyword

Always use `#define_method`, Never use the `def` keyword.

```ruby
  # Objectively inferior

  def a_method(*args)
    nil # LOL
  end

  # Objectively superior

  define_method(:a_method) { |*args|
    nil # LOL
  end
```
``ruby

### Parentheses

Obviously always use paretheses when defining a method, LOL if you don't.

## Classes

### Definition / class keyword

Never use the `class` keyword instead define classes in the following way.

```ruby
  # Objectively inferior

  class Thing
    # ...
  end

  # Objectively superior

  Thing = Class.new {
    # ...
  }
```

* When anonymous classes need to be defined your syntax is the same
* More Ruby (better language transparency / less VM Magic)
* Forces you think about classes as objects

### Re-opening

Crazy programmers like to reopen classes.
When the urge is too much, keep to the no `class` keyword rule, instead use
`#class_eval`.

```ruby
  # Objectively inferior

  class Thing
    # I re-opened u but I can't even tell LOL
  end

  # Objectively superior

  Thing.class_eval {
    # I re-opened u for sure ROFL
  }
```

* Definition and re-opening cannot be confused
* Does not use any keywords

### Constants

Never use class constants, instead define instance methods that return a new
copy of your constant literals each time.

```ruby

  # Objectively inferior

  class Thing
    VALUES = { one: "one" }.freeze
  end

  # Objectively superior

  Thing = Class.new {
    define_method(:one) {
      {
        one: "one",
      }
    }
  }
```

* Even when frozen, inner values of the constant can be mutated
* Anything at a static or class level should be avoided in an OO system