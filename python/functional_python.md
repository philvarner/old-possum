# Functional Python

## Resources

* [Functional Programming HOWTO by A. M. Kuchling](https://docs.python.org/3/howto/functional.html)
* [PyMonad](https://github.com/jasondelaat/pymonad)
* Effective Python Chapter 4 Generators and Comprehensions

## Comprehensions

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
