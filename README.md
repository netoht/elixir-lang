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

#### The match operator

We have used the `=` a couple times to assign variables. In Elixir, the `=` 
operator is actually called *the match operator*.

A variable can only be assigned on the left side of `=`.

```elixir
# assign variables
iex> x = 1
1
iex> x
1

# match operator
iex> 1 = x
1
iex> 2 = x
** (MatchError) no match of right hand side value: 1
```

Notice that `1 = x` is a valid expression, and it matched because both the left
and right side are equal to 1. When the sides do not match, a `MatchError` is raised.

#### Pattern matching

The *match operator* is not only used to match against simple values, but it is
also useful for **destructuring** more complex data types.

A pattern match will error in the case the sides can't match. This is, for example,
the case when the tuples have different sizes. And also when comparing different
types.

```elixir
# We can patten match on tuples
iex> {a, b, c} = {:hello, "world", 42}
{:hello, "world", 42}
iex> a
:hello
iex> b
"world"

# Error when the tuples have different sizes
iex> {a, b, c} = {:hello, "world"}
** (MatchError) no match of right hand side value: {:hello, "world"}

# Error when comparing different types
iex> {a, b, c} = [:hello, "world", "!"]
** (MatchError) no match of right hand side value: [:hello, "world", "!"]
```

We can match on specific values. The example below asserts that the left side
will only match the right side when the right side is a tuple that starts with
the atom `:ok`:

```elixir
iex> {:ok, result} = {:ok, 13}
{:ok, 13}
iex> result
13

iex> {:ok, result} = {:error, :oops}
** (MatchError) no match of right hand side value: {:error, :oops}
```
We can pattern match on list. A list also supports matching on its own `head`
and `tail`.

```elixir
# Pattern match on list
iex> [a, b, c] = [1, 2, 3]
[1, 2, 3]
iex> a
1

# Matching on head and tail
iex> [head | tail] = [1, 2, 3]
[1, 2, 3]
iex> head
1
iex> tail
[2, 3]
```

Similar to the `hd/1` and `tl/1` functions, we can't match an empty list with
a head and tail pattern:

```elixir
iex> [h | t] = []
** (MatchError) no match of right hand side value: []
```

#### The pin operator

Variables in Elixir can be rebound.

The pin operator `^` should be used when you want to pattern match against an
existing variable's value rather than rebinding the variable:

```elixir
iex> x = 1
1
iex> ^x = 2
** (MatchError) no match of right hand side value: 2
iex> {y, ^x} = {2, 1}
{2, 1}
iex> y
2
iex> {y, ^x} = {2, 2}
** (MatchError) no match of right hand side value: {2, 2}
```

If a variable is mentioned more than once in a pattern, all references should
bind to the same pattern:

```elixir
iex> {x, x} = {1, 1}
1
iex> {x, x} = {1, 2}
** (MatchError) no match of right hand side value: {1, 2}
```

In some cases, you don't care about particular value in a pattern. It is a common
practice to bind those values to the underscore `_`. For example, if only the
head of the list matters to us, we can assign the tail to underscore:

The variable `_` is special in that it can never be read from.

```elixir
# Only the head of the list matters to us
iex> [h | _] = [1, 2, 3]
[1, 2, 3]
iex> h
1

# Error trying to read variable underscore
iex> _
** (CompileError) iex:1: unbound variable _
```
# Control-flow structures: case, cond and if

#### case

`case` allow us to compare a value against many pattern until we find a matching
one:

```elixir
iex> case {1, 2, 3} do
...>   {4, 5, 6} ->
...>     "This clause won't match"
...>   {1, x, 3} ->
...>     "This clause will match and bind x to 2 in this clause"
...>   _ ->
...>     "This clause would match any value"
...> end
"This clause will match and bind x to 2 in this clause"
```

If you want to pattern match against an existing variable, you need to use the
`^` operator:

```elixir
iex> x = 1
1
iex> case 10 do
...>   ^x -> "Won't match"
...>   _  -> "Will match"
...> end
"Will match"
```

Clauses also allow extra conditions to be specified via guards:

```elixir
iex> case {1, 2, 3} do
...>   {1, x, 3} when x > 0 ->
...>      "Will match"
...>   _ ->
...>      "Would match, if guard condition were not satisfied"
...> end
"Will match"
```

#### Expressions in guard clauses

Elixir imports and allows the following expressions in guards by default:

- comparison operators (`==`, `!=`, `===`, `!==`, `>`, `>=`, `<`, `<=`)
- boolean operators (`and`, `or`, `not`)
- arithmetic operations (`+`, `-`, `*`, `/`)
- arithmetic unary operators (`+`, `-`)
- the binary concatenation operator `<>`
- the `in` operator as long as the right side is a range or a list
- all the following type check functions:
    - `is_atom/1`
    - `is_binary/1`
    - `is_bitstring/1`
    - `is_boolean/1`
    - `is_float/1`
    - `is_function/1`
    - `is_function/2`
    - `is_integer/1`
    - `is_list/1`
    - `is_map/1`
    - `is_nil/1`
    - `is_number/1`
    - `is_pid/1`
    - `is_port/1`
    - `is_reference/1`
    - `is_tuple/1`
- plus these functions:
    - `abs(number)`
    - `binary_part(binary, start, length)`
    - `bit_size(bitstring)`
    - `byte_size(bitstring)`
    - `div(integer, integer)`
    - `elem(tuple, n)`
    - `hd(list)`
    - `length(list)`
    - `map_size(map)`
    - `node()`
    - `node(pid | ref | port)`
    - `rem(integer, integer)`
    - `round(number)`
    - `self()`
    - `tl(list)`
    - `trunc(number)`
    - `tuple_size(tuple)`

Additionally, users may define their own guards. For example, the `Bitwise` 
module defines guards as functions and operators: `bnot`, `~~~`, `band`, `&&&`,
`bor`, `|||`, `bxor`, `^^^`, `bsl`, `<<<`, `bsr`, `>>>`.

Note that while boolean operators such as `and`, `or`, `not` are allowed in guards,
the more general and short-circuiting operators `&&`, `||` and `!` **are not**. 

If none of the clauses match, an error is raised:

```elixir
iex> case :ok do
...>   :error -> "Won't match"
...> end
** (CaseClauseError) no case clause matching: :ok
```

Note anonymous functions can also have multiple clauses and guards:

```elixir
iex> f = fn
...>   x, y when x > 0 -> x + y
...>   x, y -> x * y
...> end
#Function<12.71889879/2 in :erl_eval.expr/5>
iex> f.(1, 3)
4
iex> f.(-1, 3)
-3
```

The number arguments in each anonymous function clause needs to be the same,
otherwise an error is raised.

#### cond

`case` is useful when you need to match against different values. However, in many
circumstances, we want to check different conditions and find the first one that
evaluates to true. In such cases, one may use `cond`:

```elixir
iex> cond do
...>   2 + 2 == 5 ->
...>     "This will not be true"
...>   2 * 2 == 3 ->
...>     "Nor this"
...>   1 + 1 == 2 ->
...>     "But this will"
...> end
"But this will"
```

This is equivalent to `else if` clauses in many imperative languages (although
used way less frequently here).

If none the conditions return true, an error is raised. For this reason, it may
be necessary to add a final condition, equal to true, which will always match:

```elixir
iex> cond do
...>   2 + 2 == 5 ->
...>     "This is never true"
...>   2 * 2 == 3 ->
...>     "Nor this"
...>   true ->
...>     "This is always true (equivalent to else)"
...> end
"This is always true (equivalent to else)"
```

Note `cond` considers any value besides `nil` and `false` to be true:

```elixir
iex> cond do
...>   hd([1, 2, 3]) ->
...>     "1 is considered as true"
...> end
"1 is considered as true"
```

# STOP: http://elixir-lang.org/getting-started/case-cond-and-if.html#if-and-unless

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
