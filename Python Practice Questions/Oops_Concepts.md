Here are **12 comprehensive Python OOP practice questions** designed to give you hands-on mastery of **every major OOP concept** in Python. They explicitly cover:

- The **4 pillars** (Encapsulation, Inheritance, Abstraction, Polymorphism)
- Class & static methods
- Class variables vs instance variables
- `@property`, setters, deleters
- Method decorators & class decorators
- Dunder/magic methods (operator overloading, `__str__`, `__eq__`, etc.)
- Multiple inheritance + mixins + `super()`
- Abstract Base Classes (ABC)
- And more (singleton pattern, validation decorators, etc.)

Each question builds real-world usable code. **Implement them one by one** (or all together in a single file) and test thoroughly. I’ve included exact requirements and test cases so you can verify your solution immediately.

### Question 1: Encapsulation + `@property` (Pillar: Encapsulation)
Create a class `BankAccount` with:
- Private attributes: `__account_number` and `__balance`
- Constructor that takes account number and initial balance (must be ≥ 0)
- `@property` for `balance` (read-only)
- `@balance.setter` that validates amount ≥ 0
- Public methods: `deposit(amount)`, `withdraw(amount)` (raise `ValueError` on insufficient funds)
- Magic method `__str__` to print account details

**Test:**
```python
acc = BankAccount("ACC123", 500)
acc.deposit(300)
acc.withdraw(200)
print(acc)          # Account ACC123: Balance = ₹600.0
print(acc.balance)  # 600.0
```

### Question 2: Single & Multilevel Inheritance + Method Overriding (Pillars: Inheritance + Polymorphism)
Create:
- Base `Animal` with `eat()` and `sleep()`
- `Mammal(Animal)` that adds `give_birth()`
- `Dog(Mammal)` that overrides `eat()` and adds `bark()`

Demonstrate polymorphism with a list of animals calling `eat()`.

### Question 3: Abstract Base Class (Pillar: Abstraction)
Using `abc` module:
- Abstract class `Shape` with abstract methods `area()` and `perimeter()`
- Concrete classes `Circle` and `Rectangle` that implement both

**Test:** Create objects and call `area()` and `perimeter()` on both.

### Question 4: Multiple Inheritance + Mixins + `super()` (Diamond Problem)
- Mixin `Flyable` with `fly()`
- Mixin `Swimmable` with `swim()`
- Class `Duck` inheriting from both mixins + `Animal`
- Class `Penguin` inheriting from `Swimmable` + `Bird`

Use `super()` correctly to avoid diamond issues. Add a `move()` method that calls both fly/swim where applicable.

### Question 5: Class Variables, Class Methods, Static Methods
Class `Person`:
- Class variable `population = 0`
- `__init__` that increments `population`
- `@classmethod` `get_population()` 
- `@staticmethod` `is_adult(age)`
- `@classmethod` alternative constructor `from_birth_year(name, year)`

**Test:**
```python
p1 = Person("Alice", 25)
p2 = Person.from_birth_year("Bob", 1995)
print(Person.get_population())  # 2
print(Person.is_adult(17))      # False
```

### Question 6: `@property` with Getter, Setter, Deleter + Computed Attributes
Class `Temperature`:
- Private `_celsius`
- `@property` `celsius` + setter (validate -273.15 ≤ value)
- `@property` `fahrenheit` (computed, no setter on this one)
- `@fahrenheit.setter` that converts and updates `_celsius`
- `@celsius.deleter` that resets temperature

### Question 7: Custom Method Decorator (Validation)
Write a decorator `@validate_positive` that:
- Checks all numeric arguments are > 0 before calling the method
- Raises `ValueError` otherwise
- Works on any instance method

Apply it to a `Calculator` class with methods `add`, `divide`, `power`.

### Question 8: Class Decorator (Adds Behavior to Entire Class)
Create a class decorator `@add_greeting` that automatically adds a `greet(self)` method to any decorated class.

**Usage:**
```python
@add_greeting
class Employee:
    def __init__(self, name): ...

e = Employee("Sumit")
e.greet()   # Hello, I am Sumit!
```

### Question 9: Operator Overloading + Dunder Methods (Polymorphism)
Class `Vector` (2D):
- `__init__(x, y)`
- `__add__`, `__sub__`, `__mul__` (scalar), `__eq__`, `__lt__` (compare magnitude), `__str__`, `__repr__`, `__len__` (return magnitude rounded)

**Test:**
```python
v1 = Vector(3, 4)
v2 = Vector(1, 2)
print(v1 + v2)     # Vector(4, 6)
print(v1 * 2)      # Vector(6, 8)
print(abs(v1))     # 5.0
```

### Question 10: Singleton Pattern using Class Decorator
Create a class decorator `@singleton` that ensures only one instance of the class ever exists (use `__new__` or dictionary storage).

Apply to `DatabaseConnection` class. Prove with `is` comparison that multiple creations return the same object.

### Question 11: `__slots__` + Memory Optimization (Advanced Encapsulation)
Create class `Point` using `__slots__ = ['x', 'y']`.
Compare memory usage (using `sys.getsizeof`) with a normal class having same attributes. Also add `__init__`, properties, and `__str__`.

### Question 12: Full Integration Project (All Concepts in One)
Build a mini **Library Management System**:
- Abstract `Item` (book, magazine) with `borrow()`, `return_item()`
- `Book` and `Magazine` inherit from `Item` + mixins `DigitalItem` (for e-books)
- `Library` class with class variables, class methods, static methods
- Use properties, decorators, operator overloading (`__contains__`, `__len__`)
- Singleton for `Library` itself
- Encapsulation for all private data

**Bonus Test:** Create 5 items, borrow/return, print total borrowed using polymorphism.

---

**How to practice effectively:**
1. Write each question in a separate file or section.
2. Add `if __name__ == "__main__":` with the exact test cases I gave.
3. Run and debug until all tests pass.
4. After finishing all 12, combine them into one big file and see how cleanly you can structure the code.

These 12 questions together force you to use **every** OOP concept in Python (including decorators, static/class methods, all 4 pillars, dunder methods, ABCs, mixins, singletons, etc.). Once you complete them, you will have rock-solid mastery.

Happy coding! If you want solutions, hints, or the next level (metaclasses, descriptors, dataclasses), just say the word. 🚀
