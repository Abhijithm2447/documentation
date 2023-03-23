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
