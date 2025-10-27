# Chapter 16: Object-Oriented Programming - Class and Object Errors

## Introduction

Welcome to **Object-Oriented Programming (OOP)** - organizing code into classes and objects. OOP is fundamental to Python and most modern programming. Understanding OOP errors is essential for building robust applications.

Common errors:
- **AttributeError**: Missing attributes or methods
- **TypeError**: Wrong initialization or method calls
- **NameError**: Class/method not defined
- Inheritance issues

Let's master OOP!

---

## 16.1 Classes and Objects

### Basic Class Definition

```python
# Define a class
class Dog:
    def __init__(self, name, age):
        self.name = name  # Instance attribute
        self.age = age
    
    def bark(self):
        return f"{self.name} says woof!"
    
    def get_age(self):
        return self.age

# Create objects (instances)
dog1 = Dog("Buddy", 5)
dog2 = Dog("Max", 3)

# Access attributes
print(dog1.name)  # "Buddy"
print(dog2.age)   # 3

# Call methods
print(dog1.bark())  # "Buddy says woof!"

# Class attributes (shared by all instances)
class Cat:
    species = "Felis catus"  # Class attribute
    
    def __init__(self, name):
        self.name = name  # Instance attribute

cat1 = Cat("Whiskers")
cat2 = Cat("Mittens")
print(cat1.species)  # "Felis catus"
print(cat2.species)  # "Felis catus"
```

---

### Error Type 1: `TypeError: __init__() missing required positional argument`

**Error Message:**
```python
>>> class Dog:
...     def __init__(self, name, age):
...         self.name = name
...         self.age = age
>>> dog = Dog("Buddy")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: __init__() missing 1 required positional argument: 'age'
```

**What Happened:**
Creating object without providing all required parameters.

**Why It Happens:**
- Missing arguments in initialization
- Wrong number of arguments
- Forgetting self parameter

**Code Example - WRONG:**
```python
class Person:
    def __init__(self, name, age, city):
        self.name = name
        self.age = age
        self.city = city

# Missing arguments
person = Person("Alice")  # ERROR! Missing age and city

# Too many arguments
person = Person("Alice", 25, "NYC", "USA")  # ERROR! Too many

# Wrong argument order
person = Person(25, "Alice", "NYC")  # Wrong but no error (logic issue)

# Forgetting to pass arguments
class Car:
    def __init__(self, make, model):
        self.make = make
        self.model = model

car = Car()  # ERROR! Missing make and model
```

**Code Example - CORRECT:**
```python
class Person:
    def __init__(self, name, age, city):
        self.name = name
        self.age = age
        self.city = city

# Provide all arguments
person = Person("Alice", 25, "NYC")  # ✓

# Use default parameters
class Person:
    def __init__(self, name, age=0, city="Unknown"):
        self.name = name
        self.age = age
        self.city = city

person = Person("Alice")  # ✓ Uses defaults
person = Person("Bob", 30)  # ✓ Partial defaults
person = Person("Charlie", 35, "LA")  # ✓ All specified

# Use keyword arguments
person = Person(name="Alice", age=25, city="NYC")  # ✓ Clear

# Flexible initialization with *args, **kwargs
class FlexiblePerson:
    def __init__(self, name, **kwargs):
        self.name = name
        self.age = kwargs.get('age', 0)
        self.city = kwargs.get('city', 'Unknown')

person = FlexiblePerson("Alice")  # ✓
person = FlexiblePerson("Bob", age=30)  # ✓
person = FlexiblePerson("Charlie", age=35, city="LA")  # ✓

# Validate arguments
class Person:
    def __init__(self, name, age):
        if not name:
            raise ValueError("Name cannot be empty")
        if age < 0:
            raise ValueError("Age cannot be negative")
        self.name = name
        self.age = age  # ✓ Validated
```

---

## 16.2 Instance vs Class Attributes

### Understanding Attribute Scope

```python
class Counter:
    # Class attribute (shared)
    total_count = 0
    
    def __init__(self, name):
        # Instance attribute (unique to each object)
        self.name = name
        self.count = 0
        Counter.total_count += 1
    
    def increment(self):
        self.count += 1

# Create instances
c1 = Counter("Counter1")
c2 = Counter("Counter2")

print(Counter.total_count)  # 2 (class attribute)
print(c1.count)  # 0 (instance attribute)
print(c2.count)  # 0 (instance attribute)

c1.increment()
print(c1.count)  # 1
print(c2.count)  # 0 (unchanged)
```

---

### Error Type 2: `AttributeError: 'ClassName' object has no attribute 'attribute_name'`

**Error Message:**
```python
>>> class Dog:
...     def __init__(self, name):
...         self.name = name
>>> dog = Dog("Buddy")
>>> print(dog.age)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Dog' object has no attribute 'age'
```

**What Happened:**
Accessing attribute that doesn't exist.

**Why It Happens:**
- Attribute not defined in __init__
- Typo in attribute name
- Conditional attribute creation
- Accessing before assignment

**Code Example - WRONG:**
```python
class Person:
    def __init__(self, name):
        self.name = name
        # age not defined!

person = Person("Alice")
print(person.age)  # ERROR! age doesn't exist

# Typo
class Car:
    def __init__(self, make):
        self.make = make

car = Car("Toyota")
print(car.maker)  # ERROR! Typo: maker vs make

# Conditional creation
class Student:
    def __init__(self, name, graduated=False):
        self.name = name
        if graduated:
            self.graduation_year = 2024

student = Student("Alice")
print(student.graduation_year)  # ERROR! Not created

# Accessing class attribute on instance incorrectly
class MyClass:
    class_var = "class"

obj = MyClass()
print(MyClass.instance_var)  # ERROR! Doesn't exist
```

**Code Example - CORRECT:**
```python
class Person:
    def __init__(self, name, age=None):
        self.name = name
        self.age = age  # ✓ Always defined

person = Person("Alice")
print(person.age)  # None (but defined)

# Check attribute exists
if hasattr(person, 'age'):
    print(person.age)  # ✓
else:
    print("No age attribute")

# Use getattr with default
age = getattr(person, 'age', 0)  # ✓ Returns 0 if not exists

# Always initialize attributes
class Student:
    def __init__(self, name, graduated=False):
        self.name = name
        self.graduation_year = 2024 if graduated else None  # ✓

student = Student("Alice")
print(student.graduation_year)  # None (but defined)

# Use property with getter
class Person:
    def __init__(self, name):
        self.name = name
        self._age = None
    
    @property
    def age(self):
        return self._age if self._age is not None else 0  # ✓
    
    @age.setter
    def age(self, value):
        if value < 0:
            raise ValueError("Age cannot be negative")
        self._age = value

person = Person("Alice")
print(person.age)  # 0 (property returns default)

# Try/except for optional attributes
try:
    print(person.optional_attr)
except AttributeError:
    print("Attribute doesn't exist")  # ✓
```

---

## 16.3 Methods

### Instance, Class, and Static Methods

```python
class MyClass:
    class_var = "class variable"
    
    def __init__(self, value):
        self.value = value
    
    # Instance method (accesses self)
    def instance_method(self):
        return f"Instance: {self.value}"
    
    # Class method (accesses class, not instance)
    @classmethod
    def class_method(cls):
        return f"Class: {cls.class_var}"
    
    # Static method (no access to class or instance)
    @staticmethod
    def static_method(x, y):
        return x + y

obj = MyClass(10)

# Call methods
print(obj.instance_method())      # "Instance: 10"
print(MyClass.class_method())     # "Class: class variable"
print(MyClass.static_method(5, 3)) # 8
```

---

### Error Type 3: `TypeError: method() takes 1 positional argument but 2 were given`

**Error Message:**
```python
>>> class Dog:
...     def bark():
...         return "Woof!"
>>> dog = Dog()
>>> dog.bark()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: bark() takes 0 positional arguments but 1 was given
```

**What Happened:**
Forgetting `self` parameter in method definition.

**Why It Happens:**
- Missing self parameter
- Wrong method type
- Calling method incorrectly

**Code Example - WRONG:**
```python
class Dog:
    def __init__(self, name):
        self.name = name
    
    # Missing self
    def bark():  # ERROR! Missing self
        return "Woof!"

dog = Dog("Buddy")
dog.bark()  # ERROR! Python passes self automatically

# Wrong static method
class Calculator:
    @staticmethod
    def add(self, x, y):  # ERROR! Static methods don't use self
        return x + y

# Calling instance method on class
class Cat:
    def meow(self):
        return "Meow!"

Cat.meow()  # ERROR! Need instance
```

**Code Example - CORRECT:**
```python
class Dog:
    def __init__(self, name):
        self.name = name
    
    # Include self
    def bark(self):  # ✓ self parameter
        return f"{self.name} says Woof!"

dog = Dog("Buddy")
print(dog.bark())  # ✓

# Static method (no self)
class Calculator:
    @staticmethod
    def add(x, y):  # ✓ No self
        return x + y

print(Calculator.add(5, 3))  # ✓

# Call instance method on instance
class Cat:
    def meow(self):
        return "Meow!"

cat = Cat()
print(cat.meow())  # ✓

# Or pass instance explicitly
print(Cat.meow(cat))  # ✓ Explicit self

# Class method uses cls
class Counter:
    count = 0
    
    @classmethod
    def increment(cls):  # ✓ cls parameter
        cls.count += 1

Counter.increment()  # ✓
```

---

## 16.4 Inheritance

### Extending Classes

```python
# Base class
class Animal:
    def __init__(self, name):
        self.name = name
    
    def speak(self):
        return "Some sound"

# Derived class
class Dog(Animal):
    def speak(self):  # Override
        return "Woof!"

class Cat(Animal):
    def speak(self):  # Override
        return "Meow!"

# Use inheritance
dog = Dog("Buddy")
print(dog.name)    # "Buddy" (from Animal)
print(dog.speak()) # "Woof!" (from Dog)

# Call parent method
class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name)  # Call parent __init__
        self.breed = breed
    
    def speak(self):
        parent_sound = super().speak()
        return f"{parent_sound} and Woof!"

# Multiple inheritance
class Flyable:
    def fly(self):
        return "Flying"

class Bird(Animal, Flyable):
    def speak(self):
        return "Tweet!"

bird = Bird("Tweety")
print(bird.speak())  # "Tweet!"
print(bird.fly())    # "Flying"
```

---

### Error Type 4: `TypeError: super() argument 1 must be type, not classobj`

**What Happened:**
Issues with super() or inheritance.

**Code Example - WRONG:**
```python
# Forgetting to call parent __init__
class Animal:
    def __init__(self, name):
        self.name = name

class Dog(Animal):
    def __init__(self, breed):
        # ERROR! Not calling parent __init__
        self.breed = breed

dog = Dog("Labrador")
print(dog.name)  # AttributeError! name not set

# Wrong super() syntax (Python 2 style)
class Dog(Animal):
    def __init__(self, name, breed):
        super(Dog, self).__init__(name)  # Works but verbose
        self.breed = breed

# Circular inheritance
class A(B):
    pass

class B(A):  # ERROR! Circular
    pass
```

**Code Example - CORRECT:**
```python
# Call parent __init__
class Animal:
    def __init__(self, name):
        self.name = name

class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name)  # ✓ Python 3 syntax
        self.breed = breed

dog = Dog("Buddy", "Labrador")
print(dog.name)   # ✓ "Buddy"
print(dog.breed)  # ✓ "Labrador"

# Check inheritance
print(isinstance(dog, Dog))     # True
print(isinstance(dog, Animal))  # True
print(issubclass(Dog, Animal))  # True

# Multiple inheritance - Method Resolution Order (MRO)
class A:
    def method(self):
        return "A"

class B(A):
    def method(self):
        return "B"

class C(A):
    def method(self):
        return "C"

class D(B, C):  # ✓ B before C
    pass

d = D()
print(d.method())  # "B" (follows MRO)
print(D.__mro__)   # Shows method resolution order

# Use super() in multiple inheritance
class B(A):
    def method(self):
        result = super().method()
        return f"B > {result}"

class C(A):
    def method(self):
        result = super().method()
        return f"C > {result}"

class D(B, C):
    def method(self):
        result = super().method()
        return f"D > {result}"  # ✓ Calls through MRO
```

---

## 16.5 Special Methods (Dunder Methods)

### Magic Methods

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    # String representation
    def __str__(self):
        return f"Point({self.x}, {self.y})"
    
    def __repr__(self):
        return f"Point(x={self.x}, y={self.y})"
    
    # Arithmetic operations
    def __add__(self, other):
        return Point(self.x + other.x, self.y + other.y)
    
    def __sub__(self, other):
        return Point(self.x - other.x, self.y - other.y)
    
    # Comparison
    def __eq__(self, other):
        return self.x == other.x and self.y == other.y
    
    # Length/bool
    def __len__(self):
        return int((self.x**2 + self.y**2)**0.5)
    
    def __bool__(self):
        return self.x != 0 or self.y != 0

# Usage
p1 = Point(1, 2)
p2 = Point(3, 4)

print(p1)           # "Point(1, 2)" (uses __str__)
print(p1 + p2)      # "Point(4, 6)" (uses __add__)
print(p1 == p2)     # False (uses __eq__)
print(len(p1))      # Length
print(bool(p1))     # True

# Container methods
class MyList:
    def __init__(self):
        self.items = []
    
    def __getitem__(self, index):
        return self.items[index]
    
    def __setitem__(self, index, value):
        self.items[index] = value
    
    def __len__(self):
        return len(self.items)
    
    def __contains__(self, item):
        return item in self.items

my_list = MyList()
my_list.items = [1, 2, 3]
print(my_list[0])      # 1 (uses __getitem__)
print(2 in my_list)    # True (uses __contains__)
```

---

## 16.6 Properties

### Using @property

```python
class Temperature:
    def __init__(self, celsius):
        self._celsius = celsius
    
    @property
    def celsius(self):
        """Get temperature in Celsius"""
        return self._celsius
    
    @celsius.setter
    def celsius(self, value):
        """Set temperature in Celsius"""
        if value < -273.15:
            raise ValueError("Temperature below absolute zero")
        self._celsius = value
    
    @property
    def fahrenheit(self):
        """Get temperature in Fahrenheit"""
        return self._celsius * 9/5 + 32
    
    @fahrenheit.setter
    def fahrenheit(self, value):
        """Set temperature in Fahrenheit"""
        self.celsius = (value - 32) * 5/9

# Usage
temp = Temperature(25)
print(temp.celsius)     # 25
print(temp.fahrenheit)  # 77.0

temp.celsius = 30
print(temp.fahrenheit)  # 86.0

temp.fahrenheit = 100
print(temp.celsius)     # 37.77...
```

---

## 16.7 Practice Problems

### Problem 1: Missing __init__ Argument
```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

person = Person("Alice")
```

<details>
<summary>Click for Answer</summary>

**Error:** `TypeError: __init__() missing 1 required positional argument: 'age'`

**Fix:**
```python
class Person:
    def __init__(self, name, age=0):  # ✓ Default value
        self.name = name
        self.age = age

person = Person("Alice")  # ✓
```
</details>

---

### Problem 2: Missing self
```python
class Dog:
    def bark():
        return "Woof!"

dog = Dog()
dog.bark()
```

<details>
<summary>Click for Answer</summary>

**Error:** `TypeError: bark() takes 0 positional arguments but 1 was given`

**Fix:**
```python
class Dog:
    def bark(self):  # ✓ Add self
        return "Woof!"

dog = Dog()
print(dog.bark())  # ✓
```
</details>

---

## 16.8 Key Takeaways

### What You Learned

1. **Include self** - In all instance methods
2. **Call super().__init__()** - In derived classes
3. **Initialize all attributes** - In __init__
4. **Use @property** - For computed attributes
5. **Check with hasattr()** - Before accessing attributes
6. **Provide defaults** - For optional parameters
7. **Use isinstance()** - For type checking

### Common Patterns

```python
# Pattern 1: Basic class
class MyClass:
    def __init__(self, value):
        self.value = value
    
    def method(self):
        return self.value

# Pattern 2: Inheritance
class Child(Parent):
    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)

# Pattern 3: Property
class MyClass:
    @property
    def value(self):
        return self._value
    
    @value.setter
    def value(self, val):
        self._value = val
```

---

## 16.9 Moving Forward

You now understand OOP! In **Chapter 17**, we'll explore **Modules and Imports**!

