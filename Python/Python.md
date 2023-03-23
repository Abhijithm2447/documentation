## Python Tricks
## Classes & OOP
### Object Comparisons: “is” vs “==”
The == operator compares by checking for **equality**

The is operator, however, compares **identities**

```python 3
a = [1, 2, 3]
b = a
print(a)
# [1, 2, 3]
print(b)
# [1, 2, 3]
print(a == b)
# True
print(a is b)
# True
c = list(a)
print(a == c)
# True
print(a is c)
# False
```

## String Conversion `__repr__`
```python
class Car:
	def __init__(self, color, mileage):
		self.color = color
		self.mileage = mileage
		
	def __str__(self):
		return f'a {self.color} car'

my_car = Car('red', 37281)
print(my_car)
# 'a red car'
```
`__str__` and `__repr__`
```python
class Car:
	def __init__(self, color, mileage):
		self.color = color
		self.mileage = mileage
	def __repr__(self):
		return '__repr__ for Car'
	def __str__(self):
		return '__str__ for Car'

my_car = Car('red', 37281)
print(my_car)
# __str__ for Car
print('{}'.format(my_car))
# '__str__ for Car'
>>> my_car
__repr__ for Car
```
If you don’t add a __str__ method, Python falls back on the result of `__repr__` when looking for `__str__`. Therefore, I recommend that you always add at least a `__repr__` method to your classes.
```python
def __repr__(self):
	return f'Car({self.color!r}, {self.mileage!r})'
```
the !r conversion flag to make sure the output string uses repr(self.color) and repr(self.mileage) instead of str(self.color) and str(self.mileage)
```python
class Car:
	def __init__(self, color, mileage):
		self.color = color
		self.mileage = mileage
	def __repr__(self):
		return (f'{self.__class__.__name__}(f'{self.color!r}, {self.mileage!r})')
	def __str__(self):
		return f'a {self.color} car'
```
