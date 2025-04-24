# Python

- [Using Python as a strong, statically-typed functional language](./functional_python.ipynb)

## Resources

- Fluent Python by Luciano Ramalho
- Effective Python by Brett Slatkin
- Functional Python Programming - Third Edition by Steven F. Lott
- Supercharged Python: Take Your Code to the Next Level by Overland and Bennett
- Architecture Patterns with Python by Harry Percival, Bob Gregory
- Using Asyncio in Python by Caleb Hattingh
- Python Testing with pytest by Brian Okken
- Hypermodern Python Tooling by Claudio Jolowicz

## Projects

- [Cookiecutter Hypermodern Python](https://cookiecutter-hypermodern-python.readthedocs.io/en/2021.11.26/quickstart.html)

## Typing

- <http://mypy-lang.org/>
- <https://mypy.readthedocs.io/en/stable/cheat_sheet_py3.html>
- Pylance

## Static Analysis

- <https://pypi.org/project/pyflakes/>

## twiddle wakka

Version spec	Actual range
~= 2.4.0	>= 2.4.0 and < 2.5.0
~= 2.4	>= 2.4.0 and < 3.0
~= 2	>= 2.0 and < 3.0

## inspection

import inspect
inspect.signature() or inspect.getfullargspec()

## venv

python3 -m venv venv
. venv/bin/activate
pip install -r requirements.txt

## Tuple unpacking

```python
a, b, *rest = range(5)
a, *body, c, d = range(5)
```

can be nested also

## find

next((link for link in self.links if link.rel == rel), None)
