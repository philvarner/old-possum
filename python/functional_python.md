# Functional Python

## Resources

* [Functional Programming HOWTO by A. M. Kuchling](https://docs.python.org/3/howto/functional.html)
* [PyMonad](https://github.com/jasondelaat/pymonad)
* *Effective Python* Chapter 4 Generators and Comprehensions

## Immutability

Python has no way of declaring a variable to non-reassignable.

frozenlist
use tuple instead of list

## Basic HOFs

`map(f, xs)` and `filter(f, xs)` are built-ins. `functools.reduce(f, xs[, init])` can be used either as reduce or foldl.

## Listcomps and Genexps

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

## functools

[functools](https://docs.python.org/3/library/functools.html) docs

* reduce(function, iterable[, initializer])
* partial(func, /, *args, **keywords)

## itertools

[itertools](https://docs.python.org/3/library/itertools.html)

## Other

* from operator import mul
* itemgetter
