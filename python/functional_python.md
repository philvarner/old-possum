# Functional Programming in Python

- [Functional Programming in Python](#functional-programming-in-python)
  - [Overview](#overview)
  - [Resources](#resources)
  - [Built-in Features and Behavior](#built-in-features-and-behavior)
  - [Python Libraries for Functional Concepts](#python-libraries-for-functional-concepts)
    - [Expression](#expression)
    - [Returns](#returns)
    - [Pyrusted family](#pyrusted-family)
    - [Pyrsistent](#pyrsistent)
  - [Wrappers (monoids, monads, applicative functors, etc.)](#wrappers-monoids-monads-applicative-functors-etc)
    - [Option / Maybe](#option--maybe)
      - [Haskell](#haskell)
      - [Scala](#scala)
      - [Rust](#rust)
      - [F#](#f)
      - [Python w/ Expression](#python-w-expression)
      - [Python w/ Returns](#python-w-returns)
      - [Python w/ Pyrusted Maybe](#python-w-pyrusted-maybe)
    - [Either / Result / Try](#either--result--try)
      - [Haskell](#haskell-1)
      - [Scala](#scala-1)
      - [Rust](#rust-1)
      - [F#](#f-1)
      - [Python w/ Expression](#python-w-expression-1)
      - [Python w/ Returns](#python-w-returns-1)
      - [Python w/ Pyrusted Result](#python-w-pyrusted-result)
    - [Immutable List](#immutable-list)
      - [Haskell](#haskell-2)
      - [Scala](#scala-2)
      - [Rust](#rust-2)
      - [F#](#f-2)
      - [Python w/ Expression](#python-w-expression-2)
      - [Python w/ Returns](#python-w-returns-2)
    - [Immutable Set](#immutable-set)
      - [Haskell](#haskell-3)
      - [Scala](#scala-3)
      - [Rust](#rust-3)
      - [F#](#f-3)
      - [Python w/ Expression](#python-w-expression-3)
      - [Python w/ Returns](#python-w-returns-3)
    - [Immutable Map](#immutable-map)
      - [Haskell](#haskell-4)
      - [Scala](#scala-4)
      - [Rust](#rust-4)
      - [F#](#f-4)
      - [Python w/ Expression](#python-w-expression-4)
      - [Python w/ Returns](#python-w-returns-4)
    - [IO](#io)
      - [Haskell](#haskell-5)
      - [Scala](#scala-5)
      - [Rust](#rust-5)
      - [F#](#f-5)
      - [Python w/ Expression](#python-w-expression-5)
      - [Python w/ Returns](#python-w-returns-5)
    - [State](#state)
      - [Haskell](#haskell-6)
      - [Scala](#scala-6)
      - [Rust](#rust-6)
      - [F#](#f-6)
      - [Python w/ Expression](#python-w-expression-6)
      - [Python w/ Returns](#python-w-returns-6)
    - [Reader](#reader)
      - [Haskell](#haskell-7)
      - [Scala](#scala-7)
      - [Rust](#rust-7)
      - [F#](#f-7)
      - [Python w/ Expression](#python-w-expression-7)
      - [Python w/ Returns](#python-w-returns-7)
    - [Writer](#writer)
      - [Haskell](#haskell-8)
      - [Scala](#scala-8)
      - [Rust](#rust-8)
      - [F#](#f-8)
      - [Python w/ Expression](#python-w-expression-8)
      - [Python w/ Returns](#python-w-returns-8)
  - [Pattern Matching](#pattern-matching)
    - [Scala](#scala-9)
    - [Rust](#rust-9)
    - [F#](#f-9)
      - [Python w/ Expression](#python-w-expression-9)
      - [Python w/ Returns](#python-w-returns-9)
  - [Functions](#functions)
    - [Higher-order Functions](#higher-order-functions)
    - [Function Composition](#function-composition)
    - [Currying](#currying)
  - [Python Standard Library](#python-standard-library)
    - [Immutability](#immutability)
    - [Basic HOFs](#basic-hofs)
    - [Listcomps and Genexps](#listcomps-and-genexps)
    - [functools](#functools)
    - [itertools](#itertools)
    - [Other](#other)
  - [Other Libraries](#other-libraries)
    - [Monads](#monads)
    - [function-oriented libraries (no wrapper types)](#function-oriented-libraries-no-wrapper-types)
    - [Python extension languages](#python-extension-languages)

## Overview

What do we mean by functional programming?

- First-class and higher-order functions
- pure functions / methods
- immutable (persistent) data structures
- static typing
- data classes + functions instead of classes + methods

## Resources

- [Python Functional Programming HOWTO by A. M. Kuchling](https://docs.python.org/3/howto/functional.html)
- [Functional Programming Jargon](https://github.com/dry-python/functional-jargon-python)

## Built-in Features and Behavior

- @dataclass(eq=True, order=True, frozen=True) (instead of namedtuple)
- Final[float] (3.8+)
- structural pattern matching (3.10+)
- lambdas
- itertools
- functools
- comprehensions/generators
- persistent data structures - frozenset, frozenmap (3.9+)

## Python Libraries for Functional Concepts

### [Expression](https://github.com/cognitedata/Expression)

- Python 3.9+
- Containers - Option, Result, Try
- Pattern matching constructs for Python 3.9
- Pipelining, composition, currying
- Immutable data structures - Seq, AsyncSeq, FrozenList, Map, AsyncObservable
- Effects - option, result 
- Mailbox Processor, Cancellation Token, Disposable

### [Returns](https://github.com/dry-python/returns)

[Well-documented](https://returns.readthedocs.io/)!

- Python 3.7+
- Containers - Maybe, Result, IO, Future, Context
- Pipelining, Converters (Maybe <=> Result, flatten), pointfree, composition, currying
- do notation

### [Pyrusted family](https://github.com/rustedpy)

Copy of Rust classes

- [Maybe](https://github.com/rustedpy/maybe)
- [Result](https://github.com/rustedpy/result)

### [Pyrsistent](https://pyrsistent.readthedocs.io/en/latest/intro.html)

- Python 3.7+
- Immutable (persistent) data structures - PVector (list), PMap (dict), PSet (set),
  PRecord (PMap with fixed fields, optional type and invariant checking),
  PClass (class), PBag (collections.Counter), PList (linked list),
  PDeque (collections.deque)
- CheckedPVector, CheckedPMap and CheckedPSet
- Transformations, like [instar](https://github.com/boxed/instar/) for Clojure
- Evolvers
- freeze/thaw for standard Python interaction

## Wrappers (monoids, monads, applicative functors, etc.)

### Option / Maybe

#### Haskell

```haskell
data Maybe a = Just a | Nothing

let a = Just 1
let b = Nothing

fmap (+1) (a)  -- Just 2
fmap (+1) (b) -- Nothing  
```

#### Scala

```scala
// Option[T] is Some[T] or None

val a = Some(1) // Some(1)
val b = Option(1) // Some(1)
val c = None
val d = Option(null) // None
val e = Some(null) // Some(null)

a.map(_ + 1) // Some(2)
c.map(_ + 1) // None
```

#### Rust

```rust
// enum Option<T> {
//     Some(T)
//     Nothing
// }

let a = Some(1);
let b = Nothing;

a.map(|n| n + 1) // Some(2)
b.map(|n| n + 1) // None
```

#### F\#

```fsharp
// type Option<'a> =
//    | Some of 'a
//    | None

let a = Some 1
let b = None

a |> Option.map (fun v -> v + 1) // Some 2
b |> Option.map (fun v -> v + 1) // None
```

#### Python w/ Expression

```python
# Option[T] is Some[T] or Nothing
from expression import Option, Some, Nothing

a: Option[int] = Some(1)
b: Option[int] = Nothing

a.map(lambda n: n + 1) # Some(2)
b.map(lambda n: n + 1) # Nothing
```

#### Python w/ Returns

```python
from returns.maybe import Maybe, Some, Nothing, maybe

a1: Maybe[int] = Maybe.from_optional(1) # Some(1)
b1: Maybe[int] = Maybe.from_optional(None) # Nothing

a2: Maybe[int] = Maybe.from_value(1) # Some(1)
b2: Maybe[int] = Maybe.from_value(None) # Some(None)

a1.map(lambda x: x + 1) # Some(2)
b1.map(lambda x: x + 1) # Nothing

a2.map(lambda x: x + 1) # Some(2)
b2.map(lambda x: x + 1) # throws TypeError, None + 1 is invalid
b2.map(lambda x: x + 1 if x else None) # Some(None)

# map, but convert None to Nothing and everything else to Some
a1.bind_optional(lambda x: x + 1 if x else None) # Some(2)
b1.bind_optional(lambda x: x + 1 if x else None) # Nothing

a2.bind_optional(lambda x: x + 1 if x else None) # Some(2)
b2.bind_optional(lambda x: x + 1 if x else None) # Nothing
```

#### Python w/ Pyrusted Maybe

```python
from rustedpy-maybe import Nothing, Some, Maybe

a1: Maybe[int] = Some(1) # Some(1)
b1: Maybe[int] = Nothing() # Nothing

a1.map(lambda x: x + 1) # Some(2)
b1.map(lambda x: x + 1) # Nothing

a1.unwrap_or_else(2) # 1
b1.unwrap_or_else(2) # 2
```

### Either / Result / Try

#### Haskell

Either left or right type, right-biased

```haskell
data Either a b = Left a | Right b

let a = Right 1
let b = Left "error"

fmap (+1) (a)  -- Right 2
fmap (+1) (b) -- Left "error"
```

#### Scala

Either, right-biased

```scala
// Either[A, B] is Left[A] or Right[B], right-biased
val a: Either[String, Integer] = Right(1)
val b: Either[String, Integer] = Left("error")

a.map(_ + 1) // Right(2)
b.map(_ + 1) // Left("error")
```

Try:

```scala
// Try[A] is effectively Either[A, Throwable], Success(v) or Failure(ex), success-biased
import scala.util.{Try, Success, Failure}

var a = Success("a")  
var b = Failure(new Exception("bad"))  

var ex = Try { throw new Exception("bad") } // Failure(Exception("bad"))
```

#### Rust

More like Scala's Try than Either.

```rust
//enum Result<T, E> {
//   Ok(T),
//   Err(E),
//}

let a: Result<&str, i32> = Ok(1);
let b: Result<&str, i32> = Err("error");

a.map(|n| n + 1) // Some(2)
b.map(|n| n + 1) // Err("error")
```

#### F\#

```fsharp
// type Result<'T,'TError> =
//    | Ok of ResultValue:'T
//    | Error of ErrorValue:'TError

let a = Ok 1
let b = Error "error"

a |> Result.map (fun v -> v + 1) // Ok 2
b |> Result.map (fun v -> v + 1) // Error "error"
```

#### Python w/ Expression

```python
from expression import Error, Ok, Result

a: Result[int, str] = Ok(1)
b: Result[int, str] = Error("error")

a.map(lambda n: n + 1) # Ok(2)
b.map(lambda n: n + 1) # Error("error")
```

#### Python w/ Returns

```python
from returns.result import Result, Success, Failure
a: Result[int, str] = Success(1)
b: Result[int, str] = Failure("error")

a.map(lambda n: n + 1) # Success(2)
b.map(lambda n: n + 1) # Failure("error")
```

#### Python w/ Pyrusted Result

```python
from result import Ok, Err, Result, is_ok, is_err
a: Result[int, str] = Ok(1)
b: Result[int, str] = Err("error")

a.map(lambda n: n + 1) # Ok(2)
b.map(lambda n: n + 1) # Err("error")
```

### Immutable List

#### Haskell

```haskell
```

#### Scala
```scala
```

#### Rust
```rust
```

#### F\# 
```fsharp
```

#### Python w/ Expression
```python
```

#### Python w/ Returns

```python
```




### Immutable Set


#### Haskell

```haskell
```

#### Scala
```scala
```

#### Rust
```rust
```

#### F\# 
```fsharp
```

#### Python w/ Expression
```python
```

#### Python w/ Returns

```python
```




### Immutable Map

#### Haskell

```haskell
```

#### Scala
```scala
```

#### Rust
```rust
```

#### F\# 
```fsharp
```

#### Python w/ Expression
```python
```

#### Python w/ Returns

```python
```




### IO


#### Haskell

```haskell
```

#### Scala
```scala
```

#### Rust
```rust
```

#### F\# 
```fsharp
```

#### Python w/ Expression
```python
```

#### Python w/ Returns

```python
```



### State


#### Haskell

```haskell
```

#### Scala
```scala
```

#### Rust
```rust
```

#### F\# 
```fsharp
```

#### Python w/ Expression
```python
```

#### Python w/ Returns

```python
```




### Reader


#### Haskell

```haskell
```

#### Scala
```scala
```

#### Rust
```rust
```

#### F\# 
```fsharp
```

#### Python w/ Expression
```python
```

#### Python w/ Returns

```python
```




### Writer

#### Haskell

```haskell
```

#### Scala
```scala
```

#### Rust
```rust
```

#### F\# 
```fsharp
```

#### Python w/ Expression
```python
```

#### Python w/ Returns

```python
```




## Pattern Matching

### Scala 

```scala
// Option[T] is Some[T] or None
val s = Some(1)

s match {
    case Some(x) => println(s"$x")
    case None    => println("no value")
}
```

### Rust
```rust
let a = Some(1);
match a {
    Some(x) => print("answer: ", x),
    Nothing => print("no value"),
}
```

### F\#

```fsharp
match validInt with
| Some x -> printfn "the valid value is %A" x
| None -> printfn "the value is None"
```

#### Python w/ Expression
```python
# Option[T] is Some[T] or Nothing
from expression import Option, Some, Nothing, match

a: Option[int] = Some(1)
b: Option[int] = Nothing

with match(a) as case:
    for value in case(Some[int]):
        print("answer: ", x)

    if case._: # Some not int or Nothing
        print("no value")


for value in a.match(Some):
    print(f"value: {value}")

print(a.map(lambda n: n + 1))
```

#### Python w/ Returns

```python
from dataclasses import dataclass
from returns.maybe import Maybe, Some, Nothing, maybe

@dataclass
class Thing(object):
    name: str

match Some(Thing(name="bike")):
    case Some(Thing(name='bike')):
        print('bike matched')
    case Some(t):
        print(f"another thing matched: {t}")
    case Nothing: # or case _:
        print('Nothing matched')
```

## Functions

### Higher-order Functions


### Function Composition


### Currying


## Python Standard Library

### Immutability

frozenlist
use tuple instead of list

### Basic HOFs

`map(f, xs)` and `filter(f, xs)` are built-ins. `functools.reduce(f, xs[, init])` can be used either as reduce or foldl.

### Listcomps and Genexps

These take the place of most uses of map and filter.

list:
```python
[x*2 for x in xs if x % 2 == 0]
```

map:
```python
{x:x*2 for x in xs if x % 2 == 0}
```

set:
```python
{x*2 for x in xs if x % 2 == 0}
```

generator:
```python
(x*2 for x in xs if x % 2 == 0)
```

### functools

[functools](https://docs.python.org/3/library/functools.html) docs

- reduce(function, iterable[, initializer])
- partial(func, /, *args, **keywords)

### itertools

[itertools](https://docs.python.org/3/library/itertools.html)

### Other

- from operator import mul
- itemgetter


## Other Libraries

### Monads

- [PyMonad](https://github.com/jasondelaat/pymonad) - Not well-documented, no commits for a year.

### function-oriented libraries (no wrapper types)

- [Toolz](https://github.com/pytoolz/toolz) and [CyToolz](https://github.com/pytoolz/cytoolz/) "A set of utility functions for iterators, functions, and dictionaries."
- [Funcy](https://github.com/Suor/funcy) "A collection of fancy functional tools focused on practicality."
- [More Itertools](https://github.com/more-itertools/more-itertools) " In more-itertools we collect additional building blocks, recipes, and routines for working with Python iterables."

### Python extension languages

- [Coconut](http://coconut-lang.org) "is a variant of Python that adds on top of Python syntax new features for simple, elegant, Pythonic functional programming."
