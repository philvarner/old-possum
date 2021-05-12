# Python

## Resources 

* Fluent Python by Luciano Ramalho
* Effective Python by Brett Slatkin

## Typing

* http://mypy-lang.org/
* https://mypy.readthedocs.io/en/stable/cheat_sheet_py3.html
* Pylance

## Static Analysis

* https://pypi.org/project/pyflakes/

## twiddle wakka

Version spec	Actual range
~> 2.4.0	>= 2.4.0 and < 2.5.0
~> 2.4	>= 2.4.0 and < 3.0
~> 2	>= 2.0 and < 3.0

## inspection

import inspect
inspect.signature() or inspect.getfullargspec()

## venv

python3 -m venv venv
. venv/bin/activate
pip install -r requirements.txt 