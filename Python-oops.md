# 🐍 Python OOP & Advanced Topics Reference

A **comprehensive guide** for Python developers covering **Object-Oriented Programming (OOP)** and advanced features, with explanations and examples. Perfect for beginners and intermediate learners.

---

## 🏗️ 1. Classes & Objects

**Description:**  
A class is a blueprint for objects. Objects are instances of classes containing **attributes** (data) and **methods** (functions).

```python
# Define a simple class
class Person:
    def __init__(self, name, age):
        self.name = name    # attribute
        self.age = age

    def greet(self):
        print(f"Hello, my name is {self.name}")

# Create object
p1 = Person("Alice", 30)
p1.greet()  # Output: Hello, my name is Alice
````

---

## 🔐 2. Encapsulation

**Description:**
Encapsulation restricts access to some components of an object. Use **private attributes** to hide data.

```python
class BankAccount:
    def __init__(self, balance):
        self.__balance = balance  # private variable

    def deposit(self, amount):
        self.__balance += amount

    def get_balance(self):
        return self.__balance

account = BankAccount(100)
account.deposit(50)
print(account.get_balance())  # Output: 150
```

---

## 🧩 3. Inheritance

**Description:**
Inheritance allows a class to **derive properties and methods** from another class. This enables code reuse.

```python
class Employee(Person):  # Inherits from Person
    def __init__(self, name, age, role):
        super().__init__(name, age)
        self.role = role

    def greet(self):
        print(f"Hello, I'm {self.name}, working as {self.role}")

e = Employee("Bob", 28, "Developer")
e.greet()
```

### Multiple Inheritance

```python
class Flyer:
    def fly(self):
        print("Flying high!")

class Bird(Flyer, Person):
    pass

b = Bird("Sparrow", 2)
b.fly()  # Output: Flying high!
```

---

## 🌀 4. Polymorphism

**Description:**
Polymorphism allows objects of different classes to be treated **uniformly** if they implement the same methods.

```python
class Cat:
    def speak(self):
        print("Meow")

class Dog:
    def speak(self):
        print("Woof")

def animal_sound(animal):
    animal.speak()

c = Cat()
d = Dog()
animal_sound(c)  # Meow
animal_sound(d)  # Woof
```

---

## ⚙️ 5. Abstraction (ABC)

**Description:**
Abstraction hides implementation details. Use **abstract classes** and **abstract methods** to define a common interface.

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return 3.14 * self.radius ** 2

c = Circle(5)
print(c.area())  # Output: 78.5
```

---

## ✨ 6. *args & **kwargs

**Description:**
`*args` allows passing **variable number of positional arguments**.
`**kwargs` allows passing **variable number of keyword arguments**.

```python
def greet(*names):
    for name in names:
        print(f"Hello {name}")

greet("Alice", "Bob", "Charlie")

def print_info(**info):
    for key, value in info.items():
        print(f"{key}: {value}")

print_info(name="Alice", age=30)
```

---

## 🏷️ 7. Decorators

**Description:**
Decorators **wrap functions or methods** to extend behavior without modifying their code.

### Function Decorator

```python
def decorator(func):
    def wrapper():
        print("Before function call")
        func()
        print("After function call")
    return wrapper

@decorator
def say_hello():
    print("Hello!")

say_hello()
```

### Decorator with Arguments

```python
def repeat(n):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for _ in range(n):
                func(*args, **kwargs)
        return wrapper
    return decorator

@repeat(3)
def greet(name):
    print(f"Hi {name}!")

greet("Alice")  # Hi Alice! printed 3 times
```

---

## 🔹 8. Classmethods & Staticmethods

**Description:**

* `@staticmethod`: method independent of object or class.
* `@classmethod`: method that receives the class as first argument.

```python
class Math:
    @staticmethod
    def add(a, b):
        return a + b

    @classmethod
    def info(cls):
        print(f"This is the {cls.__name__} class")

print(Math.add(5, 3))
Math.info()
```

---

## 🔑 9. Properties

**Description:**
`@property` allows **controlled access** to attributes, enabling getters and setters.

```python
class Temperature:
    def __init__(self, celsius):
        self._celsius = celsius

    @property
    def celsius(self):
        return self._celsius

    @celsius.setter
    def celsius(self, value):
        self._celsius = value

t = Temperature(25)
print(t.celsius)
t.celsius = 30
print(t.celsius)
```

---

## ⚡ 10. Dunder / Magic Methods

**Description:**
Special methods like `__add__`, `__str__`, `__repr__` **customize Python behavior** for operators and built-ins.

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)

    def __str__(self):
        return f"Vector({self.x}, {self.y})"

v1 = Vector(2, 3)
v2 = Vector(4, 1)
print(v1 + v2)  # Vector(6, 4)
```

---

## 🔁 11. Iterators & Generators

**Description:**

* **Iterators** implement `__iter__` and `__next__`.
* **Generators** use `yield` to produce values lazily.

### Iterator

```python
class MyNumbers:
    def __init__(self):
        self.num = 1

    def __iter__(self):
        return self

    def __next__(self):
        if self.num <= 5:
            n = self.num
            self.num += 1
            return n
        else:
            raise StopIteration

numbers = MyNumbers()
for n in numbers:
    print(n)
```

### Generator

```python
def my_gen():
    for i in range(5):
        yield i

for val in my_gen():
    print(val)
```

---

## 🧹 12. Context Managers

**Description:**
Use context managers (`with`) to **ensure proper resource management**, e.g., file handling.

```python
with open("file.txt", "w") as f:
    f.write("Hello World")

# Custom context manager
class ManagedFile:
    def __init__(self, filename):
        self.filename = filename

    def __enter__(self):
        self.file = open(self.filename, "w")
        return self.file

    def __exit__(self, exc_type, exc_val, exc_tb):
        self.file.close()

with ManagedFile("hello.txt") as f:
    f.write("Python OOP!")
```

---

## 🏛️ 13. Metaclasses

**Description:**
Metaclasses define **how classes are created**, allowing advanced customization.

```python
class Meta(type):
    def __new__(cls, name, bases, dct):
        print(f"Creating class {name}")
        return super().__new__(cls, name, bases, dct)

class MyClass(metaclass=Meta):
    pass

# Output: Creating class MyClass
```

---
