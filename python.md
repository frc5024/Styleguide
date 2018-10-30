# Python
Python is used by the team for computer vision, webservers, automated tools, machine learning, git hooks, and linting.

## Versions
The preferred version of python to use is python 3.6 but, python 3.7 and 3.7.1 are also allowed. Older versions are not allowed due to their lack of support from machine learning and computer vision libraries.

## Indentation
Due to python's strict rules on indentation, we must pick sides in the ultimate war. And so we have. Everything must be indented using tabs. Code reviews and pull requests will be rejected if spaces are used.

## Function Naming
All functions are to be named using camelCase. For example:
```python
# Good
def winGame():
	print("Let's Go!")

# Bad
def WinGame():
	print("Let's Go!")
```

## Type Hints
All functions must use type hints. Both for parameters and return values. Type hints can only be ignored when the function can take in or produce many types of data. Here is an example of using type hints to concatenate strings:
```python
def concatStrings(first_string: str, second_string: str) -> str:
	return first_string + second_string
```

Further documentation on using type hints can be founr here: https://docs.python.org/3/library/typing.html

## Classes
When naming a class, All words must be uppercase. For example:
```python
# Good
class FourCubeAuto(object):

# Bad
class FourCubeAuto(object):
```

When creating a class, use python's `__init__` function unless your code will only be run using python37. In that case, use dataclasses.

Classes must also be documented using docstrings. Their methods should also be documented in the same way.

Here is a full example of a class:
```python
class Team(object):
	""" Store data about a team """
	
	__init__(self, number: int, name: str):
		self.team_number = number
		self.team_name = name
		
	def getName(self):
		""" Returns the name of the team """
		return self.team_name
	
	def willWin(self):
		""" Returns true if they will win everything... again """
		if self.team_number == 254:
			return true
		else:
			return False
```

## If Else
We use python's standard `if` and `else` statements, but instead of using `else if ... :` in a statement, we use `elif ... :`

## Infinite loops
There are many ways to do an infinite loop in python, our preferred method is:
```python
while True:
	doThings()
```
