# Elixir Lang

In this article we are going to learn the Elixir foundation, the language 
syntax, how to define modules, how to manipulate the characteristics of common 
data structures and more.

# Our requirements are

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

#### Elixir basic types

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
(i.e. **arity**).
Therefore, `is_boolean/1` identifies a function named `is_boolean` that takes 
1 argument. `is_boolean/2` identifies a different (noexistent) function with 
the same name but different arity.

#### Atoms

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

#### Strings

[Strings](http://elixir-lang.org/docs/stable/elixir/String.html) in Elixir are:

```elixir
# Inserted between double quotes and they are encoded in UTF-8
iex> "hellö"
"hellö"

# Supports string interpolation
iex> "hellö #{:world}"
"hellö world"

# String concatenation is done with <>
iex> "foo" <> "bar"
"foobar"

# Can have line breaks in them
iex> "hello
...> world"
"hello\nworld"
iex> "hello\nworld"
"hello\nworld"

# Represented internally by binaries which are sequences of bytes
iex> is_binary("hellö")
true

# Getting the number of bytes
iex> byte_size("hellö")
6

# Notice the number of bytes in that string is 6, even though it has
# 5 characters. That's because the character “ö” takes 2 bytes to be 
# represented in UTF-8

# can get the actual length of the string
iex> String.length("hellö")
5
```

#### Anonymous functions

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

Functions are "first class citizens" in Elixir meaning they can be passed as 
arguments to other functions just as integers and strings can.

> Note a dot (.) between the variable and parenthesis is required to invoke an
anonymous function.

Anonymous functions are **closures**, and as such they can access variables that
are in scope when the function is defined:

```elixir
iex> add_two = fn a -> add.(a, 2) end
#Function<6.71889879/1 in :erl_eval.expr/5>
iex> add_two.(2)
4
```

A variable assigned inside a function does not affect itts surrounding 
environment:

```elixir
iex> x = 42
42
iex> (fn -> x = 0 end).()
0
iex> x
42
```

The capture syntax [`&()`](http://elixir-lang.org/docs/stable/elixir/Kernel.SpecialForms.html#&/1) can also be used for creating anonymous functions.

```elixir
iex> add = &(&1 + &2)
&:erlang.+/2
iex> add.(1, 2)
3
```

#### (Linked) Lists

Elixir uses **square brackets []** to specify a list of values. Values can be of any
type.

Two lists can be concatenated and subtracted using the `++/2` and `--/2` operators.

The `head` is a first element of a list and the `tail` is the remainder of a list.
They can be retrieved with the functions `hd/1` and `tl/1`.

Getting the `head` or `tail` of an empty list is an error.

```elixir
# Defining a List
iex> [1, 2, true, 3]
[1, 2, true, 3]
iex> length [1, 2, 3]
3

# Concatenation and subtraction
iex> [1, 2, 3] ++ [4, 5, 6]
[1, 2, 3, 4, 5, 6]
iex> [1, true, 2, false, 3, true] -- [true, false]
[1, 2, 3, true]

# Head and Tail of a list
iex> list = [1, 2, 3]
iex> hd(list)
1
iex> tl(list)
[2, 3]

# Getting the head or tail of a empty list is an error
iex> hd []
** (ArgumentError) argument error
```

Sometimes you will create a list and it will return a value is single-quotes.

```elixir
iex> [11, 12, 13]
'\v\f\r'
iex> [104, 101, 108, 108, 111]
'hello'
```

When Elixir sees a list of printable ASCII numbers, Elixir will print that as a
char list (literally a list of characters). Char lists are quite common when
interfacing with existing Erlang code. Whenever you see a value in `IEx` and you
are not quite sure what it is, you can use the `i/1` to retrieve information
about it:

```
iex> i 'hello'
Term
  'hello'
Data type
  List
Description
  ...
Raw representation
  [104, 101, 108, 108, 111]
Reference modules
  List
```

Keep in mind `single-quoted` and `double-quoted` representations are **not 
equivalent** in Elixir as they are represented by different types:

- `Single-quotes` are **char lists**
- `Double-quotes` are **strings**

```elixir
iex> 'hello' == "hello"
false
```

#### Tuples

Elixir uses **curly brackets {}** to define tuples. Like lists, tuples *can hold
any value*.

Tuples store elements contiguously in memory. This means accessing a tuple 
element per index or getting the tuple size is a *fast operation* (**indexes start
from zero**).

```elixir
# Defining a Tuple
iex> {:ok, "hello"}
{:ok, "hello"}
iex> tuple_size {:ok, "hello"}
2

# Accessing a tuple element
iex> tuple = {:ok, "hello"}
{:ok, "hello"}
iex> elem(tuple, 1)
"hello"
iex> tuple_size(tuple)
2
```

It is also possible to put an element at a particular index in a tuple with 
`put_elem/3`.

```elixir
iex> tuple = {:ok, "hello"}
{:ok, "hello"}
iex> put_elem(tuple, 1, "world")
{:ok, "world"}
iex> tuple
{:ok, "hello"}
```

Notice that `put_elem/3` **returned a new tuple**. The original tuple stored in
the `tuple` variable was not modified because **Elixir data types are immutable**, 
Elixir code is easier to reason about as you never need to worry if a particular
code is mutating your data structure in place.

#### Lists or Tuples?

What is the difference between `lists` and `tuples`?

`Lists` are stored in memory as linked lists, meaning that each element in a list
holds its value and points to the following element until the end of the list is
reached. We call each pair of value and pointer a **cons cell**:

```elixir
iex> list = [1 | [2 | [3 | []]]]
[1, 2, 3]
```

This means accessing the length of a list is a linear operation: we need to 
traverse the whole list in order to figure out its size. Updating a list is fast
as long as we are prepending elements:

```elixir
iex> [0 | list]
[0, 1, 2, 3]
```

`Tuples`, on the other hand, are stored contiguously in memory. This means getting
the tuple size or accessing an element by index is fast. However, updating or
adding elements to tuples is **expensive** because it requires copying the whole
tuple in memory.

Those performance characteristics dictate the usage of those data structures.
One very common use case for tuples is to use them to return extra information
from a function. For example, `File.read/1` is a function that can be used to
read file contents and it returns tuples:

```elixir
iex> File.read("path/to/existing/file")
{:ok, "... contents ..."}
iex> File.read("path/to/unknown/file")
{:error, :enoent}
```

If the path given to `File.read/1` exists, it returns a tuple with the atom `:ok`
as the first element and the file contents as the second. Otherwise, it returns
a tuple with :error and the error description.

Most of the time, Elixir is going to guide you to do the right thing. For example,
there is a `elem/2` function to access a tuple item but there is no built-in
equivalent for lists:

```elixir
iex> tuple = {:ok, "hello"}
{:ok, "hello"}
iex> elem(tuple, 1)
"hello"
```

When "counting" the number of elements in a data structure, Elixir also abides
by a simple rule: the function is a named `size` if the operation is in constant
time (i.e. the value is pre-calculated) or `length` if the operation is linear
(i.e. calculating the length gets slower as the input grows).

For example, we have used 4 counting functions so far: `byte_size/1` (for the
number of bytes in a string), `tuple_size/1` (for the tuple size), `length/1` 
(for the list length) and `String.length/1` (for the number of graphemes in a
string). That said, we use `byte_size` to get the number of bytes in a string,
which is cheap, but retrieving the number of unicode characters use `String.length`,
since the whole string needs to be traversed.

Elixir also provides `Port`, `Reference` and `PID` as data types (usually used
in process communication).

# Basic operators

Elixir provides three boolean operators: `or`, `and` (both are **short-circuit**
operators) and `not`.

Besides these boolean operators, Elixir also provides `||`, `&&` and `!` which
*accept arguments of any type*. For these operators, all values except `false`
and `nil` will evaluate to `true`.

As a rule of thumb, use `and`, `or` and `not` when you are expecting booleans.
If any of the arguments are `non-boolean`, use `&&`, `||` and `!`.

Providing a `non-boolean` will raise an exception.

```elixir
iex> true and true
true
iex> false or is_atom(:example)
true

# non-boolean
iex> 1 and true
** (ArgumentError) argument error

# or
iex> 1 || true
1
iex> false || 11
11

# and
iex> nil && 13
nil
iex> true && 17
17

# !
iex> !true
false
iex> !1
false
iex> !nil
true
```

Elixir also provides `==`, `!=`, `===`, `!==`, `<=`, `>=`, `<` and `>` as 
comparison operators.

The difference between `==` and `===` is that the latter is more strict when
comparing integers and floats.

```elixir
iex> 1 == 1
true
iex> 1 != 2
true
iex> 1 < 2
true

# The difference between == and ===
iex> 1 == 1.0
true
iex> 1 === 1.0
false

# We can compare two different data types
iex> 1 < :atom
true
```

The reason we can compare different data types is pragmatism. Sorting algorithms
don't need to worry about different data types in order to sort. The overall
sorting order is defined below:

```elixir
number < atom < reference < functions < port < pid < tuple < maps < list < bitstring
```

#### Operator table

The complete operator table for Elixir ordered from **higher to lower precedence
for reference**:

| OPERATOR                                           | ASSOCIATIVITY |
|:-------------------------------------------------- |:------------- |
| `@`                                                | Unary         |
| `.`                                                | Left to right |
| `+` `-` `!` `^` `not` `~~~`                        | Unary         |
| `*` `/`                                            | Left to right |
| `+` `-`                                            | Left to right |
| `++` `--` `..` `<>`                                | Right to left |
| `in`                                               | Left to right |
| `|>` `<<<` `>>>` `~>>` `<<~` `~>` `<~` `<~>` `<|>` | Left to right |
| `<` `>` `<=` `>=`                                  | Left to right |
| `==` `!=` `=~` `===` `!==`                         | Left to right |
| `&&` `&&&` `and`                                   | Left to right |
| `||` `|||` `or`                                    | Left to right |
| `=`                                                | Right to left |
| `=>`                                               | Right to left |
| `|`                                                | Right to left |
| `::`                                               | Right to left |
| `when`                                             | Right to left |
| `<-` `\\`                                          | Left to right |
| `&`                                                | Unary         |

# Pattern matching

# STOP: http://elixir-lang.org/getting-started/pattern-matching.html

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
