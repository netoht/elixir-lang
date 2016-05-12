# Elixir Lang

In this article we are going to learn the Elixir foundation, the language 
syntax, how to define modules, how to manipulate the characteristics of common 
data structures and more.

# Our requirements are:

- Elixir
- Erlang

# Running scripts

This can be accomplished by putting the following Elixir code in a file:

```elixir
IO.puts "Hello" <> " World"
```

Save the code above as [`simple.exs`](simple.exs) and execute it with elixir:

```sh
$ elixir simple.exs
Hello World
```

# Access documentation (iex)

At any moment you can type `h` in the shell `(iex)` to print information on how
to use the shell. 

The `h` helper can also be used to access documentation for any **function**. 
It also works with **operators** and other **constructs**.

For example, typing `h is_integer/1` is going to print the documentation for 
the `is_integer/1` function.

```elixir
iex(1)> h div

                              def div(left, right)

Performs an integer division.

Raises an ArithmeticError exception if one of the arguments is not an integer.

Allowed in guard tests. Inlined by the compiler.

Examples

┃ iex> div(5, 2)
┃ 2
```

# Basic types

#### Elixir basic types:

```elixir
iex> 1          # integer
iex> 0x1F       # integer
iex> 1.0        # float
iex> true       # boolean
iex> :atom      # atom / symbol
iex> "elixir"   # string
iex> [1, 2, 3]  # list
iex> {1, 2, 3}  # tuple
```

#### Basic arithmetic

```elixir
iex> 1 + 2
3
iex> 5 * 5
25
iex> 10 / 2
5.0
```

In Elixir, the operator `/` always returns a `float`. If you want to do 
`integer` division or get the division remainder, you can invoke the `div` 
and `rem` functions:

```elixir
iex> div(10, 2)
5
iex> rem 10, 3
1
```

Notice that parentheses are not required in order to invoke a function.

Elixir also supports shortcut notations for entering `binary`, `octal` and 
`hexadecimal` numbers:

```elixir
# binary
iex> 0b1010
10

# octal
iex> 0o777
511

# hexadecimal
iex> 0x1F
31
```

Float numbers require *a dot followed* by at least one digit and also support `e` 
for the `exponent` number:

```elixir
iex> 1.0
1.0
iex> 1.0e-10
1.0e-10
```

Floats in Elixir are 64 bit double precision.

You can invoke `round` function to get the closest integer to a given float, or 
the `trunc` function to get the integer part of a float.

```elixir
iex> round(3.58)
4
iex> trunc(3.58)
3
```

#### Booleans

Elixir provides a bunch of predicate functions to check for a value type.
For example, the `is_boolean/1` function can be used to check if a value is a 
boolean or not:

```elixir
iex> is_boolean(true)
true
iex> is_boolean(1)
false
```

You can also use `is_integer/1`, `is_float/1` or `is_number/1` to check,
respectively, if an argument is an integer, a float or either.

> Note: Functions in Elixir are identified by name and by number of arguments 
(i.e. arity).
Therefore, `is_boolean/1` identifies a function named `is_boolean` that takes 
1 argument. `is_boolean/2` identifies a different (noexistent) function with 
the same name but different arity.

# Atoms

Atoms are constants where their name is their own value. Some other languages
call these symbols:

```elixir
iex> :hello
:hello
iex> :hello == :world
false
```

The booleans `true` and `false` are, in fact, atoms:

```elixir
iex> true == :true
true
iex> is_atom(false)
true
iex> is_boolean(:false)
true
```

# Strings

[Strings](http://elixir-lang.org/docs/stable/elixir/String.html) in Elixir are:

```elixir
# inserted between double quotes and they are encoded in UTF-8
iex> "hellö"
"hellö"

# also supports string interpolation
iex> "hellö #{:world}"
"hellö world"

# can have line breaks in them
iex> "hello
...> world"
"hello\nworld"
iex> "hello\nworld"
"hello\nworld"

# represented internally by binaries which are sequences of bytes
iex> is_binary("hellö")
true

# also get the number of bytes
iex> byte_size("hellö")
6

# Notice the number of bytes in that string is 6, even though it has
# 5 characters. That's because the character “ö” takes 2 bytes to be 
# represented in UTF-8

# can get the actual length of the string
iex> String.length("hellö")
5
```

# Anonymous functions

Functions are delimited by the keywords `fn` and `end`:

```elixir
iex> add = fn a, b -> a + b end
#Function<12.71889879/2 in :erl_eval.expr/5>
iex> is_function(add)
true
iex> is_function(add, 2)
true
iex> is_function(add, 1)
false
iex> add.(1, 2)
3
```

# STOP: http://elixir-lang.org/getting-started/basic-types.html#anonymous-functions

# References

- [Install](http://elixir-lang.org/install.html)
- [Introduction](http://elixir-lang.org/getting-started/introduction.html)
- [Mix & OPT guide](http://elixir-lang.org/getting-started/mix-otp/introduction-to-mix.html)
- []()

# Asking questions

- [#elixir-lang on freenode IRC](irc://irc.freenode.net/elixir-lang)
- [Elixir on Slack](https://elixir-slackin.herokuapp.com/)
- [Elixir Forum](http://elixirforum.com/)
- [elixir-talk mailing list](https://groups.google.com/group/elixir-lang-talk)
- [elixir tag on StackOverflow](https://stackoverflow.com/questions/tagged/elixir)
