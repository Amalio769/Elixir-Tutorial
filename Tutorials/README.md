# Elixir-Tutorial

- To see what version you have installed: `elixir –version` or  `elixir -v`
- Interactive mode → `iex` (to exit Ctrl+C twice)
- Running scripts. Write in a file (simple.exs) the following → IO.puts(“Hello world from Elixir”)
elixir simple.exs
- Notice that Elixir allows you to drop the parentheses when invoking named functions with at least one argument. This feature gives a cleaner syntax when writing declarations and control-flow constructs. However, Elixir developers generally prefer to use parentheses.
```Elixir
iex> 10/2
5
iex> div(10, 2)
5
iex> div 10, 2
5
```
- Elixir also supports shortcut notations for entering binary, octal, and hexadecimal numbers:
```Elixir
iex> 0b1010
10
iex> 0o777
511
iex> 0x1F
31
```
- Float numbers require a dot followed by at least one digit and also support e for scientific notation:
```Elixir
iex> 1.0
1.0
iex> 1.0e-1
0.1
```
> **Functions in Elixir are identified by both their name and their arity**.
- The arity of a function describes the number of arguments that the function takes. From this point on we will use both the function name and its arity to describe functions throughout the documentation. `trunc/` identifies the function which is named trunc and takes 1 argument, whereas `trunc/2` identifies a different (nonexistent) function with the same name but with an arity of 2.
- The Elixir shell defines the `h` funtion, which you can use to access documentation for any function. For example, typing `h trunc/1` is going to print the documentation for the trunc/1 function.

## Booleans
> **Elixir supports `true` and `false` as booleans**
- Elixir provides a bunch of predicate functions to check for a value type. For example, the `is_boolean/1` function can be used to check if a value is a boolean or not
- You can also use `is_integer/1`, `is_float/1` or `is_number/1` to check, respectively, if an argument is an integer, a float, or either.

## Atoms
> **An atom is a constant whose value is its own name**.
- Atoms are equal if their names are equal.
- Often they are used to express the state of an operation, by using values such as `:ok` and `:error`.
- The booleans `true` and `false` are also atoms.
- Elixir allows you to skip the leading `:` for the atoms `false`, `true` and `nil`.
- **Aliases** start in upper case and are also atoms
```Elixir
iex> is_atom(Hello)
true
```

## Strings
> **Strings in Elixir are delimited by double quotes, and they are encoded in UTF-8**
- Elixir also supports string interpolation
```Elixir
iex> string = :world
iex> "hellö #{string}"
"hellö world"
```
- Strings can have line breaks in them. You can introduce them using escape sequences:
```Elixir
iex> "hello
...> world"
"hello\nworld"
iex> "hello\nworld"
"hello\nworld"
```
- You can print a string using the `IO.puts/1` function from the IO module:
```Elixir
iex> IO.puts("hello\nworld")
hello
world
:ok
```
- Notice that the `IO.puts/1` function returns the atom `:ok` after printing.
- Strings in Elixir are represented internally by contiguous sequences of bytes known as binaries:
```Elixir
iex> is_binary("hellö")
true
```
- We can also get the number of bytes in a string:
```Elixir
iex> byte_size("hellö")
6
```
- Notice that the number of bytes in that string is 6, even though it has 5 graphemes. That’s because the grapheme “ö” takes 2 bytes to be represented in UTF-8. We can get the actual length of the string, based on the number of graphemes, by using the `String.length/1` function:
```Elixir
iex> String.length("hellö")
5
```
- The String module contains a bunch of functions that operate on strings as defined in the Unicode standard:
```Elixir
iex> String.upcase("hellö")
"HELLÖ"
```

## Anonymous functions
- Elixir also provides anonymous functions. Anonymous functions allow us to store and pass executable code around as if it was an integer or a string. They are delimited by the keywords fn and end:
```Elixir
iex> add = fn a, b -> a + b end
#Function<12.71889879/2 in :erl_eval.expr/5>
iex> add.(1, 2)
3
iex> is_function(add)
true
```
- In the example above, we defined an anonymous function that receives two arguments, `a` and `b`, and returns the result of `a + b`. The arguments are always on the left-hand side of `->` and the code to be executed on the right-hand side. The anonymous function is stored in the variable `add`.
- We can invoke anonymous functions by passing arguments to it. Note that a dot (.) between the variable and parentheses is required to invoke an anonymous function. The dot ensures there is no ambiguity between calling the anonymous function matched to a variable add and a named function add/2. We will write our own named functions when dealing with Modules and Functions. For now, just remember that Elixir makes a clear distinction between anonymous functions and named functions.

## Lists
> **Elixir uses square brackets to specify a list of values. Values can be of any type**:
```Elixir
iex> [1, 2, true, 3]
[1, 2, true, 3]
iex> length [1, 2, 3]
3
```
- Two lists can be concatenated or subtracted using the `++/2` and `--/2` operators respectively:
```Elixir
iex> [1, 2, 3] ++ [4, 5, 6]
[1, 2, 3, 4, 5, 6]
iex> [1, true, 2, false, 3, true] -- [true, false]
[1, 2, 3, true]
```
> **List operators never modify the existing list. Concatenating to or removing elements from a list returns a new list. We say that Elixir data structures are immutable. One advantage of immutability is that it leads to clearer code. You can freely pass the data around with the guarantee no one will mutate it in memory - only transform it**.
- Throughout the tutorial, we will talk a lot about the `head` and `tail` of a list. The head is the first element of a list and the tail is the remainder of the list. They can be retrieved with the functions `hd/1` and `tl/1`. Let’s assign a list to a variable and retrieve its head and tail:
```Elixir
iex> list = [1, 2, 3]
iex> hd(list)
1
iex> tl(list)
[2, 3]
```
- Sometimes you will create a list and it will return a quoted value preceded by `~c`.
When Elixir sees a list of printable ASCII numbers, Elixir will print that as a charlist (literally a list of characters). Charlists are quite common when interfacing with existing Erlang code. Whenever you see a value in IEx and you are not quite sure what it is, you can use the `i/1` to retrieve information about it
- Keep in mind single-quoted and double-quoted representations are not equivalent in Elixir as they are represented by different types:
```Elixir
iex> 'hello' == "hello"
false
iex> 'hello' == ~c"hello"
true
```
> **Single quotes are charlists, double quotes are strings**.

## Tuples
> **Elixir uses curly brackets to define tuples. Like  lists, tuples can hold any value**:
```Elixir
iex> {:ok, "hello"}
{:ok, "hello"}
iex> tuple_size {:ok, "hello"}
2
```
- Tuples store elements contiguously in memory. This means accessing a tuple element by index or getting the tuple size is a fast operation. Indexes start from zero:
```Elixir
iex> tuple = {:ok, "hello"}
{:ok, "hello"}
iex> elem(tuple, 1)
"hello"
iex> tuple_size(tuple)
2
```
- It is also possible to put an element at a particular index in a tuple with `put_elem/3`:
```Elixir
iex> tuple = {:ok, "hello"}
{:ok, "hello"}
iex> put_elem(tuple, 1, "world")
{:ok, "world"}
iex> tuple
{:ok, "hello"}
```
- Notice that `put_elem/3` returned a new tuple. The original tuple stored in the tuple variable was not modified. 
> **Like lists, tuples are also immutable. Every operation on a tuple returns a new tuple, it never changes the given one**.