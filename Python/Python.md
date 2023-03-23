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

