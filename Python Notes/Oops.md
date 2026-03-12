**OOP Concepts in Python – Complete Notes**  
(Updated for Python 3.12+ style – clean, modern, with type hints)

I have divided the notes into **8 core topics**.  
For **every topic** you will get:  
- Clear explanation  
- **3 practical examples** (ready to copy-paste and run)  

At the **end** I have given **5 comprehensive examples** that combine **ALL topics** together with detailed line-by-line explanations.

---

### 1. Classes and Objects
**Explanation**: Class = blueprint/template. Object = real instance created from the blueprint. Every object has its own data (attributes) and behaviour (methods).

**Example 1 – Basic Class**
```python
class Dog:
    def bark(self):
        return "Woof!"

d1 = Dog()          # object creation
print(d1.bark())    # Woof!
```

**Example 2 – Constructor (`__init__`)**
```python
class Person:
    def __init__(self, name: str, age: int):
        self.name = name
        self.age = age

p = Person("Sumit", 25)
print(p.name, p.age)   # Sumit 25
```

**Example 3 – Instance vs Class Variable**
```python
class Student:
    school = "ABC School"          # class variable (shared)
    def __init__(self, name):
        self.name = name           # instance variable (unique)

s1 = Student("Alice")
s2 = Student("Bob")
print(s1.school, s2.school)   # both ABC School
```

---

### 2. Encapsulation (Data Hiding)
**Explanation**: Bundling data + methods and restricting direct access. In Python we use `_` (protected by convention) and `__` (name mangled private).

**Example 1 – Single Underscore (protected)**
```python
class Bank:
    def __init__(self):
        self._balance = 1000   # protected

b = Bank()
print(b._balance)   # 1000 (can still access, but convention says don't)
```

**Example 2 – Double Underscore (private + name mangling)**
```python
class Account:
    def __init__(self):
        self.__balance = 5000

a = Account()
# print(a.__balance)        # AttributeError
print(a._Account__balance)  # 5000 (forced access)
```

**Example 3 – Encapsulation with Property (Best Practice)**
```python
class Temperature:
    def __init__(self, celsius: float):
        self._celsius = celsius
    
    @property
    def celsius(self):
        return self._celsius
```

---

### 3. Inheritance
**Explanation**: Child class reuses code from parent class (`super()` is used to call parent methods).

**Example 1 – Single Inheritance**
```python
class Animal:
    def eat(self): return "Eating..."

class Dog(Animal):
    def bark(self): return "Woof!"

d = Dog()
print(d.eat())   # Eating...
```

**Example 2 – Multilevel Inheritance**
```python
class Vehicle: pass
class Car(Vehicle): pass
class SportsCar(Car): pass
```

**Example 3 – Using `super()`**
```python
class Parent:
    def __init__(self): print("Parent init")

class Child(Parent):
    def __init__(self):
        super().__init__()
        print("Child init")

Child()   # prints both
```

---

### 4. Polymorphism
**Explanation**: Same method name, different behaviour (overriding + operator overloading).

**Example 1 – Method Overriding**
```python
class Animal:
    def sound(self): return "Some sound"

class Dog(Animal):
    def sound(self): return "Woof!"

class Cat(Animal):
    def sound(self): return "Meow!"

for animal in [Dog(), Cat()]:
    print(animal.sound())
```

**Example 2 – Operator Overloading**
```python
class Vector:
    def __init__(self, x): self.x = x
    def __add__(self, other):
        return Vector(self.x + other.x)

v1 = Vector(5)
v2 = Vector(3)
print((v1 + v2).x)   # 8
```

**Example 3 – Duck Typing (Python style)**
```python
def make_sound(animal):
    print(animal.sound())   # works for any object that has .sound()
```

---

### 5. Abstraction
**Explanation**: Hide implementation details using Abstract Base Classes (`abc` module).

**Example 1 – Abstract Class**
```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self): pass
```

**Example 2 – Concrete Implementation**
```python
class Circle(Shape):
    def __init__(self, r): self.r = r
    def area(self): return 3.14 * self.r ** 2
```

**Example 3 – Cannot instantiate abstract class**
```python
# s = Shape()   # TypeError
c = Circle(5)
print(c.area())
```

---

### 6. Class Methods & Static Methods
**Explanation**:  
- `@classmethod` → works on class (first arg `cls`)  
- `@staticmethod` → no self/cls, utility function inside class

**Example 1 – Class Method (Factory)**
```python
class Person:
    population = 0
    def __init__(self): Person.population += 1
    
    @classmethod
    def get_population(cls):
        return cls.population
```

**Example 2 – Static Method**
```python
class Math:
    @staticmethod
    def add(a, b): return a + b
```

**Example 3 – Alternative Constructor**
```python
class Date:
    @classmethod
    def from_string(cls, date_str):
        y, m, d = map(int, date_str.split('-'))
        return cls(y, m, d)
```

---

### 7. @property (Getter, Setter, Deleter)
**Explanation**: Makes methods behave like attributes.

**Example 1 – Simple Property**
```python
class Circle:
    def __init__(self, r): self._radius = r
    
    @property
    def radius(self): return self._radius
```

**Example 2 – Setter + Validation**
```python
    @radius.setter
    def radius(self, value):
        if value < 0: raise ValueError
        self._radius = value
```

**Example 3 – Deleter**
```python
    @radius.deleter
    def radius(self):
        print("Deleting radius")
        del self._radius
```

---

### 8. Magic Methods (Dunder Methods)
**Explanation**: Methods with `__` that Python calls automatically.

**Example 1 – `__str__` & `__repr__`**
```python
class Book:
    def __str__(self): return "Nice Book"
    def __repr__(self): return "Book()"
```

**Example 2 – Comparison (`__eq__`, `__lt__`)**
```python
    def __eq__(self, other): return self.price == other.price
```

**Example 3 – Operator Overloading (`__add__`)**
```python
    def __add__(self, other): return Book(self.price + other.price)
```

---

### **5 Comprehensive Examples (All Topics Combined)**

**Example 1: Advanced BankAccount (Encapsulation + Property + Dunder + Class Method)**
```python
class BankAccount:
    total_accounts = 0
    
    def __init__(self, acc_no: str, balance: float = 0.0):
        if balance < 0: raise ValueError
        self.__acc_no = acc_no          # private
        self.__balance = balance
        BankAccount.total_accounts += 1
    
    @property
    def balance(self): return self.__balance
    
    @balance.setter
    def balance(self, v): raise AttributeError("Use deposit/withdraw")
    
    def deposit(self, amt): 
        if amt > 0: self.__balance += amt
    
    def __str__(self): return f"Acc {self.__acc_no} → ₹{self.__balance:,.2f}"
    
    @classmethod
    def get_total_accounts(cls): return cls.total_accounts

acc = BankAccount("SBI123", 5000)
print(acc)                     # uses __str__
acc.deposit(1000)
print(acc.balance)             # uses @property
```

**Example 2: Shape Hierarchy (Inheritance + Polymorphism + Abstraction + Magic Methods)**
```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self): pass
    
    def __add__(self, other):                # polymorphism
        return self.area() + other.area()

class Circle(Shape):
    def __init__(self, r): self.r = r
    def area(self): return 3.14 * self.r**2

class Rectangle(Shape):
    def __init__(self, w, h): self.w, self.h = w, h
    def area(self): return self.w * self.h

c = Circle(5)
r = Rectangle(4, 6)
print(c.area(), r.area())
print(c + r)          # 78.5 + 24 = 102.5 (operator overloading)
```

**Example 3: Employee System (All 4 Pillars + Class/Static Methods + Property)**
```python
from abc import ABC, abstractmethod

class Employee(ABC):
    company = "xAI"                    # class variable
    
    def __init__(self, name, salary):
        self.name = name
        self._salary = salary          # protected
    
    @property
    def salary(self): return self._salary
    
    @classmethod
    def get_company(cls): return cls.company
    
    @staticmethod
    def is_valid_salary(s): return s > 0
    
    @abstractmethod
    def get_bonus(self): pass          # abstraction

class Manager(Employee):
    def get_bonus(self): return self.salary * 0.2   # polymorphism

m = Manager("Sumit", 100000)
print(m.salary)                    # property
print(Manager.get_company())       # class method
print(Employee.is_valid_salary(50000))  # static method
```

**Example 4: Vector2D (Polymorphism + Dunder Methods + Operator Overloading)**
```python
class Vector2D:
    def __init__(self, x, y):
        self.x, self.y = x, y
    
    def __add__(self, other):
        return Vector2D(self.x + other.x, self.y + other.y)
    
    def __mul__(self, scalar):
        return Vector2D(self.x * scalar, self.y * scalar)
    
    def __str__(self):
        return f"Vector({self.x}, {self.y})"
    
    def __eq__(self, other):
        return self.x == other.x and self.y == other.y

v1 = Vector2D(3, 4)
v2 = Vector2D(1, 2)
print(v1 + v2)      # Vector(4, 6)
print(v1 * 2)       # Vector(6, 8)
print(v1 == Vector2D(3,4))
```

**Example 5: Library Management System (Everything Combined)**
```python
from abc import ABC, abstractmethod

class Item(ABC):                    # Abstraction
    def __init__(self, title):
        self._title = title
        self.__is_borrowed = False   # private
    
    @property
    def title(self): return self._title
    
    @abstractmethod
    def get_type(self): pass
    
    def borrow(self):
        if not self.__is_borrowed:
            self.__is_borrowed = True
            print(f"{self.title} borrowed!")
    
    def __str__(self): return f"{self.get_type()}: {self.title}"   # magic

class Book(Item):
    def get_type(self): return "Book"
    def __add__(self, other): return "Combined reading list"   # polymorphism

class Magazine(Item):
    def get_type(self): return "Magazine"

class Library:
    total_books = 0
    
    @classmethod
    def add_book(cls): cls.total_books += 1

b = Book("Python Mastery")
m = Magazine("Tech Today")
print(b)               # uses __str__
b.borrow()
Library.add_book()
print(Library.total_books)
```







Here is an **updated and more detailed version** of the OOP concepts notes in Python — with special emphasis on **Duck Typing**, **Concrete Implementation**, **Dunder methods** (magic methods), and clearer explanations of related concepts.

### OOP Concepts in Python – Detailed Notes (2025 style)

#### 1. Classes and Objects

Class = blueprint  
Object = instance created from the blueprint

```python
class Car:
    wheels = 4                      # class variable (shared)

    def __init__(self, brand, color):
        self.brand = brand          # instance variable
        self.color = color

    def drive(self):
        return f"{self.color} {self.brand} is driving"
```

#### 2. Encapsulation

Protecting data + controlled access  
Python levels:

| Convention       | Syntax             | Real protection level              | Common use case                             |
|------------------|--------------------|-------------------------------------|---------------------------------------------|
| public           | `self.x`           | none                                | normal attributes                           |
| protected        | `self._x`          | convention only                     | "internal – don't touch from outside"       |
| private          | `self.__x`         | name mangling (_Class__x)           | avoid accidental override in subclasses     |
| property         | `@property`        | controlled getter/setter            | validation, computation, future-proofing    |

#### 3. Inheritance

Child class re-uses parent code.

```python
class Animal:
    def __init__(self, name):
        self.name = name

class Cat(Animal):
    def speak(self):
        return "Meow"
```

#### 4. Polymorphism – very important in Python

**Same interface → different behavior**

Two main styles in Python:

**A. Classic inheritance polymorphism (method overriding)**

```python
class Bird:
    def move(self):
        return "flying"

class Penguin(Bird):
    def move(self):
        return "waddling"
```

**B. Duck typing (most Pythonic & powerful)**

**"If it walks like a duck and quacks like a duck, it is a duck."**

→ You don't care about the class/type — you care only whether it has the method/attribute you need.

```python
def make_it_move(creature):
    print(creature.move())          # doesn't care what creature is

make_it_move(Bird())                # flying
make_it_move(Penguin())             # waddling
make_it_move( Airplane() )          # works too if it has .move()
```

Duck typing examples – very common in real Python code:

```python
len("hello")             # __len__ exists → works
len([1,2,3])             # __len__ exists → works
len({"a":1, "b":2})      # __len__ exists → works

for x in range(10):      # __iter__ exists
for x in [1,2,3]:        # __iter__ exists
for x in "abc":          # __iter__ exists
```

#### 5. Abstraction

Hide **how** something works → show only **what** it can do.

Two main ways in Python:

**A. Abstract Base Class (abc module)** – most formal way

```python
from abc import ABC, abstractmethod

class Payment(ABC):
    @abstractmethod
    def pay(self, amount: float):
        pass

    @abstractmethod
    def refund(self, amount: float):
        pass
```

**B. Concrete implementation** = class that actually provides the missing methods

```python
class CreditCard(Payment):
    def pay(self, amount: float):
        print(f"Charging {amount} to credit card")

    def refund(self, amount: float):
        print(f"Refunding {amount} to credit card")

# This works:
visa = CreditCard()
visa.pay(1500)

# This fails (good – forces implementation):
# p = Payment()                    # TypeError: Can't instantiate abstract class
```

#### 6. Magic methods / Dunder methods (very detailed)

Methods with double underscore `__method__`  
Python calls them automatically in special situations.

Most important groups:

| Category               | Important dunder methods                              | When Python calls it                                  | Typical return value       |
|-----------------------|--------------------------------------------------------|-------------------------------------------------------|-----------------------------|
| String representation | `__str__`, `__repr__`                                  | `print(obj)`, `str(obj)`, `repr(obj)`                 | `str`                       |
| Comparison            | `__eq__`, `__ne__`, `__lt__`, `__le__`, `__gt__`, `__ge__` | `==`, `!=`, `<`, `<=`, `>`, `>=`                 | `bool`                      |
| Arithmetic            | `__add__`, `__sub__`, `__mul__`, `__truediv__`, `__pow__` | `+`, `-`, `*`, `/`, `**`                         | same type as self (usually) |
| Length / Container    | `__len__`, `__getitem__`, `__setitem__`, `__contains__` | `len()`, `obj[i]`, `obj[i]=v`, `v in obj`            | `int` / value / `bool`      |
| Boolean context       | `__bool__`                                             | `if obj:`, `bool(obj)`                                | `bool`                      |
| Context manager       | `__enter__`, `__exit__`                                | `with obj:`                                           | —                           |
| Iterator              | `__iter__`, `__next__`                                 | `for x in obj:`                                       | self / next value           |

**Realistic example – Vector with many dunder methods**

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __str__(self):
        return f"Vector({self.x}, {self.y})"

    def __repr__(self):
        return f"Vector(x={self.x}, y={self.y})"

    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)

    def __eq__(self, other):
        return self.x == other.x and self.y == other.y

    def __abs__(self):
        return (self.x ** 2 + self.y ** 2) ** 0.5

v1 = Vector(3, 4)
v2 = Vector(1, 2)

print(v1)               # Vector(3, 4)
print(v1 + v2)          # Vector(4, 6)
print(abs(v1))          # 5.0
print(v1 == Vector(3,4)) # True
```

#### 7. @property / getter / setter / deleter

Transforms method → attribute-like access

```python
class Employee:
    def __init__(self, name, salary):
        self.name = name
        self._salary = salary

    @property
    def salary(self):
        return self._salary

    @salary.setter
    def salary(self, value):
        if value < 0:
            raise ValueError("Salary cannot be negative")
        self._salary = value

    @salary.deleter
    def salary(self):
        print("Salary deleted")
        del self._salary
```

#### Summary Table – Quick Reference

| Concept              | Python idiom / keyword                     | Most important feature in Python                     |
|----------------------|--------------------------------------------|-------------------------------------------------------|
| Polymorphism         | method overriding + **duck typing**        | Duck typing is more powerful & common                 |
| Abstraction          | `ABC`, `@abstractmethod`                   | Forces concrete implementation                        |
| Encapsulation        | `_`, `__`, `@property`                     | `@property` is preferred way in modern code           |
| Dunder methods       | `__str__`, `__add__`, `__len__`, `__eq__`  | Makes your class feel like built-in type              |
| Concrete class       | normal class that implements all abstract methods | Required to be usable                                 |

If you want, I can now give you:

- 8–10 medium/hard exercises focused especially on **duck typing**, **concrete classes**, and **dunder methods**
- One big realistic project combining most of these concepts

Which direction would you like to go next?
