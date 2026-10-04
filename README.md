# 🏗️ Creational Design Patterns in Python

> **A practical guide to mastering GoF Creational Design Patterns with real-world analogies, problems, bad designs, improved designs, projects, and SOLID connections.**

---

## 📌 What are Design Patterns?

A **Design Pattern** is a reusable solution to a commonly occurring software design problem.

It is **not a ready-made piece of code**.

Instead, it is a **general design idea** that can be implemented differently depending on the programming language and problem.

### Simple definition

> A Design Pattern is a proven, reusable approach for solving a recurring software design problem.

---

# 📚 Gang of Four (GoF)

The famous book:

**Design Patterns: Elements of Reusable Object-Oriented Software**

was written by:

* Erich Gamma
* Richard Helm
* Ralph Johnson
* John Vlissides

They are commonly known as the **Gang of Four (GoF)**.

The book introduced **23 classic design patterns**.

These patterns are divided into three categories:

```text
                 DESIGN PATTERNS
                       │
          ┌────────────┼────────────┐
          │            │            │
     Creational    Structural   Behavioral
          │
          │
     How objects
     are created
```

---

# 🏭 Creational Design Patterns

Creational patterns deal with **object creation**.

Instead of allowing object creation logic to become scattered throughout an application, these patterns provide better ways to create objects.

The five GoF Creational Patterns are:

1. Singleton
2. Factory Method
3. Abstract Factory
4. Builder
5. Prototype

---

# 🧠 Master Mental Model

The easiest way to remember all five:

| Pattern          | Main Question         | Core Idea                 |
| ---------------- | --------------------- | ------------------------- |
| Singleton        | **How many objects?** | Only ONE instance         |
| Factory Method   | **Which object?**     | Subclass decides          |
| Abstract Factory | **Which family?**     | Related objects together  |
| Builder          | **How to build?**     | Step-by-step construction |
| Prototype        | **Can I copy one?**   | Clone existing object     |

### One-line memory trick

```text
Singleton        → ONE
Factory Method   → WHICH?
Abstract Factory → WHICH FAMILY?
Builder          → HOW?
Prototype        → COPY?
```

---

# 1️⃣ Singleton Pattern

## 📖 Definition

> **Singleton ensures that a class has only one instance and provides a way to access that instance.**

The important point is:

```text
Many references
       ↓
   ONE object
```

It does **not** mean that the object cannot change.

It means only **one actual instance** exists.

---

## 🌍 Real-World Analogy

### College Principal

Imagine a college should have only **one principal** at a time.

Many people may refer to the principal:

```text
Student ─────┐
Faculty ─────┼──→ Principal
Staff ───────┘
```

There are many references to the principal, but there is only one principal object.

---

# ❌ Before Singleton

Suppose we create a Logger:

```python
class Logger:
    def log(self, message):
        print("[LOG]:", message)


logger1 = Logger()
logger2 = Logger()

print(logger1 is logger2)
```

Output:

```text
False
```

Two different Logger objects were created.

If the application expects one centralized logger, this can cause problems with shared state and configuration.

---

# ✅ After Singleton

```python
class Logger:
    _instance = None

    def __new__(cls):

        if cls._instance is None:
            cls._instance = super().__new__(cls)

        return cls._instance

    def log(self, message):
        print("[LOG]:", message)


logger1 = Logger()
logger2 = Logger()

print(logger1 is logger2)
```

Output:

```text
True
```

Both variables refer to the same object.

```text
logger1 ──┐
          ├──→ Logger Object
logger2 ──┘
```

---

## 🔍 Important Python Concept

### `__new__()`

`__new__()` controls **object creation**.

### `__init__()`

`__init__()` initializes an object **after it has been created**.

For Singleton, we use `__new__()` because we need to control whether a new object should be created.

---

## 💻 Real-World Project

### Project: Application Logger

Build a centralized logging system.

Features:

```text
Application
     │
     ├── Login Module
     ├── Payment Module
     ├── Database Module
     └── Notification Module
             │
             ↓
          Logger
             │
             ↓
        application.log
```

### Requirements

* Only one Logger instance.
* Every module uses the same Logger.
* Store logs.
* Add timestamps.
* Support INFO, WARNING and ERROR.
* Save logs to a file.

### Why Singleton?

We want a centralized logging object shared by the application.

### Learning outcome

You learn:

* `__new__`
* object identity
* shared state
* instance control
* global-state concerns

---

## ⚠️ Singleton Warning

Singleton can be overused.

Problems can include:

* Hidden global state
* Difficult testing
* Tight coupling
* Unexpected shared state

Therefore:

> Use Singleton when exactly one shared instance is genuinely required.

---

# 2️⃣ Factory Method Pattern

## 📖 Definition

> **Factory Method defines a method for creating objects, while allowing subclasses to decide which concrete object should be created.**

The key idea:

```text
Parent
  │
  │ defines creation method
  ↓
Subclass decides
  │
  ├── Product A
  ├── Product B
  └── Product C
```

---

# 🌍 Real-World Analogy

### Restaurant

Imagine a restaurant application.

The general restaurant knows:

```text
Take Order
     ↓
Create Food
     ↓
Prepare Food
     ↓
Serve Food
```

But different restaurants decide what food they create.

```text
PizzaRestaurant  → Pizza
BurgerRestaurant → Burger
```

The overall process remains the same.

---

# ❌ Before Factory Method

```python
class Restaurant:

    def order_food(self, food_type):

        if food_type == "pizza":
            food = Pizza()

        elif food_type == "burger":
            food = Burger()

        elif food_type == "sushi":
            food = Sushi()

        food.prepare()
```

As new food types are added, the class keeps changing.

```text
Pizza
Burger
Sushi
Taco
Pasta
...
```

This can violate the **Open/Closed Principle (OCP)**.

---

# ✅ After Factory Method

```python
class Pizza:

    def prepare(self):
        print("Preparing Pizza")


class Burger:

    def prepare(self):
        print("Preparing Burger")


class Restaurant:

    def order_food(self):

        food = self.create_food()

        food.prepare()

    def create_food(self):
        raise NotImplementedError


class PizzaRestaurant(Restaurant):

    def create_food(self):
        return Pizza()


class BurgerRestaurant(Restaurant):

    def create_food(self):
        return Burger()
```

Usage:

```python
pizza_restaurant = PizzaRestaurant()
pizza_restaurant.order_food()

burger_restaurant = BurgerRestaurant()
burger_restaurant.order_food()
```

Output:

```text
Preparing Pizza
Preparing Burger
```

---

## 🧠 Why is this better?

The parent class controls the common process:

```python
def order_food(self):

    food = self.create_food()
    food.prepare()
```

The subclass decides:

```python
def create_food(self):
    return Pizza()
```

or:

```python
def create_food(self):
    return Burger()
```

---

## ➕ Adding Sushi

We don't need to modify `Restaurant.order_food()`.

```python
class Sushi:

    def prepare(self):
        print("Preparing Sushi")


class SushiRestaurant(Restaurant):

    def create_food(self):
        return Sushi()
```

This follows the **Open/Closed Principle**:

> Open for extension, closed for modification.

---

## 💻 Real-World Project

### Project: Payment Processing System

Create a payment system supporting:

```text
Payment
   │
   ├── Credit Card
   ├── UPI
   ├── PayPal
   └── Cryptocurrency
```

### Before Pattern

```python
if payment_type == "upi":
    UPI()

elif payment_type == "card":
    CreditCard()

elif payment_type == "paypal":
    PayPal()
```

### After Pattern

Create:

```text
PaymentProcessor
       │
       ├── UPIPaymentProcessor
       ├── CardPaymentProcessor
       └── PayPalPaymentProcessor
```

Each subclass decides which payment object to create.

### Learning outcome

You learn:

* Factory Method
* inheritance
* polymorphism
* OCP
* dependency on abstractions

---

# 3️⃣ Abstract Factory Pattern

## 📖 Definition

> **Abstract Factory provides an interface for creating families of related objects without specifying their concrete classes.**

The key word is:

# FAMILY

---

# 🌍 Real-World Analogy

### Furniture Store

Imagine furniture comes in different styles.

### Modern Family

```text
ModernFactory
    │
    ├── ModernChair
    ├── ModernTable
    └── ModernSofa
```

### Traditional Family

```text
TraditionalFactory
    │
    ├── TraditionalChair
    ├── TraditionalTable
    └── TraditionalSofa
```

The products belong together.

---

# ❌ Before Abstract Factory

Suppose an application supports Windows and Mac UI.

```python
if os == "windows":
    button = WindowsButton()
    checkbox = WindowsCheckbox()

elif os == "mac":
    button = MacButton()
    checkbox = MacCheckbox()
```

As components increase:

```text
Button
Checkbox
Menu
Textbox
Dialog
Slider
...
```

the conditional logic becomes difficult to maintain.

---

# ✅ After Abstract Factory

```python
class WindowsButton:

    def click(self):
        print("Windows Button clicked")


class WindowsCheckbox:

    def check(self):
        print("Windows Checkbox checked")


class MacButton:

    def click(self):
        print("Mac Button clicked")


class MacCheckbox:

    def check(self):
        print("Mac Checkbox checked")
```

Factory interface:

```python
class GUIFactory:

    def create_button(self):
        raise NotImplementedError

    def create_checkbox(self):
        raise NotImplementedError
```

Windows family:

```python
class WindowsFactory(GUIFactory):

    def create_button(self):
        return WindowsButton()

    def create_checkbox(self):
        return WindowsCheckbox()
```

Mac family:

```python
class MacFactory(GUIFactory):

    def create_button(self):
        return MacButton()

    def create_checkbox(self):
        return MacCheckbox()
```

Client:

```python
factory = WindowsFactory()

button = factory.create_button()
checkbox = factory.create_checkbox()

button.click()
checkbox.check()
```

Output:

```text
Windows Button clicked
Windows Checkbox checked
```

Switching:

```python
factory = MacFactory()
```

gives the Mac family.

---

## 🧠 Factory Method vs Abstract Factory

This is extremely important.

### Factory Method

Creates **one type of product**.

```text
Restaurant
    │
    └── create_food()
             │
             ├── Pizza
             └── Burger
```

### Abstract Factory

Creates **multiple related products**.

```text
WindowsFactory
    │
    ├── WindowsButton
    └── WindowsCheckbox
```

---

## 💻 Real-World Project

### Project: Cross-Platform UI System

Build a UI library supporting:

```text
Windows
Mac
Linux
```

Each platform has:

```text
Button
Checkbox
Textbox
Menu
```

Structure:

```text
GUIFactory
    │
    ├── WindowsFactory
    ├── MacFactory
    └── LinuxFactory
```

Each factory creates its own consistent UI family.

### Learning outcome

You learn:

* Abstract Factory
* product families
* polymorphism
* dependency inversion
* OCP
* cross-platform architecture

---

# 4️⃣ Builder Pattern

## 📖 Definition

> **Builder separates the construction of a complex object from its final representation and allows the object to be constructed step-by-step.**

The key question:

> **HOW should I build this object?**

---

# 🌍 Real-World Analogy

### Building a House

You don't build a house with one huge action.

You construct it step-by-step:

```text
Foundation
    ↓
Walls
    ↓
Roof
    ↓
Doors
    ↓
Windows
    ↓
Interior
```

The final result is one house.

---

# ❌ Before Builder

Imagine a computer with many configuration options:

```python
class Computer:

    def __init__(
        self,
        cpu,
        ram,
        storage,
        gpu,
        motherboard,
        cooling,
        power_supply
    ):
        ...
```

Creating it becomes difficult:

```python
computer = Computer(
    "Intel i7",
    "16GB",
    "1TB SSD",
    "RTX 5070",
    "ASUS",
    "Liquid Cooling",
    "750W"
)
```

As options increase, the constructor becomes difficult to understand and maintain.

---

# ✅ After Builder

```python
class Computer:

    def __init__(self):

        self.cpu = None
        self.ram = None
        self.storage = None
        self.gpu = None

    def show_specs(self):

        print("CPU:", self.cpu)
        print("RAM:", self.ram)
        print("Storage:", self.storage)
        print("GPU:", self.gpu)
```

Builder:

```python
class ComputerBuilder:

    def __init__(self):

        self.computer = Computer()

    def set_cpu(self, cpu):

        self.computer.cpu = cpu

        return self

    def set_ram(self, ram):

        self.computer.ram = ram

        return self

    def set_storage(self, storage):

        self.computer.storage = storage

        return self

    def set_gpu(self, gpu):

        self.computer.gpu = gpu

        return self

    def build(self):

        return self.computer
```

Usage:

```python
builder = ComputerBuilder()

computer = (
    builder
    .set_cpu("Intel i7")
    .set_ram("16GB")
    .set_storage("1TB SSD")
    .set_gpu("RTX 5070")
    .build()
)

computer.show_specs()
```

Output:

```text
CPU: Intel i7
RAM: 16GB
Storage: 1TB SSD
GPU: RTX 5070
```

---

# 🔍 Why `return self`?

Consider:

```python
builder.set_cpu("Intel i7")
```

The method returns the same builder.

Therefore:

```python
builder.set_cpu("Intel i7").set_ram("16GB")
```

is possible.

This is called **method chaining**.

---

# 💻 Real-World Project

### Project: Custom PC Builder

Create an application where users can configure:

```text
CPU
RAM
GPU
Storage
Motherboard
Cooling
Power Supply
Operating System
```

Users should be able to create:

### Gaming PC

```text
CPU     → Intel i7
RAM     → 32GB
GPU     → RTX 5070
Storage → 2TB SSD
```

### Office PC

```text
CPU     → Intel i5
RAM     → 16GB
GPU     → Integrated
Storage → 512GB SSD
```

Same builder, different configurations.

### Learning outcome

You learn:

* step-by-step construction
* optional parameters
* method chaining
* separation of construction from representation
* complex object creation

---

# 5️⃣ Prototype Pattern

## 📖 Definition

> **Prototype creates new objects by copying an existing object instead of constructing each object from scratch.**

The key question:

> **Can I copy an existing object instead of creating one from scratch?**

---

# 🌍 Real-World Analogy

### Photocopier

Suppose you have a document containing:

```text
Name
Address
Company
Formatting
Images
Tables
```

Instead of creating every document from scratch:

```text
Create document
Add formatting
Add images
Add tables
...
```

you make a copy of an existing document.

That's the basic idea of Prototype.

---

# ❌ Before Prototype

Imagine a game creates many similar characters.

```python
warrior1 = Character(
    "Warrior",
    100,
    "Sword"
)

warrior2 = Character(
    "Warrior",
    100,
    "Sword"
)

warrior3 = Character(
    "Warrior",
    100,
    "Sword"
)
```

Every time we manually specify the configuration.

If the object contains many properties:

```text
Name
Health
Weapon
Armor
Skills
Abilities
Position
Animation
Equipment
...
```

creation can become expensive and repetitive.

---

# ✅ After Prototype

Create one configured prototype:

```python
import copy


class Character:

    def __init__(self, name, health, weapon):

        self.name = name
        self.health = health
        self.weapon = weapon

    def clone(self):

        return copy.copy(self)
```

Create prototype:

```python
warrior = Character(
    "Warrior",
    100,
    "Sword"
)
```

Clone it:

```python
warrior2 = warrior.clone()
warrior3 = warrior.clone()
```

Now:

```python
print(warrior is warrior2)
```

Output:

```text
False
```

They are different objects.

---

# 🧠 Prototype Visualization

```text
             Prototype
                 │
                 ↓
          ┌─────────────┐
          │   Warrior   │
          │ Health: 100 │
          │ Weapon:Sword│
          └─────────────┘
             /       \
            /         \
           ↓           ↓
      Clone 1       Clone 2
      Warrior       Warrior
```

They start with copied data but are separate objects.

---

# 🆚 Singleton vs Prototype

### Singleton

```text
a ──┐
b ──┼──→ SAME object
c ──┘
```

```python
a is b
```

Output:

```text
True
```

### Prototype

```text
prototype → Object A

clone1 ───→ Object B
clone2 ───→ Object C
```

```python
prototype is clone1
```

Output:

```text
False
```

### Memory trick

> **Singleton = SAME**

> **Prototype = COPY**

---

# 🔍 Shallow Copy vs Deep Copy

Python provides:

```python
copy.copy()
copy.deepcopy()
```

---

## Shallow Copy

```python
copy.copy()
```

Creates a new outer object, but nested mutable objects can still be shared.

Example:

```python
import copy

original = {
    "name": "Warrior",
    "skills": ["Attack", "Defend"]
}

clone = copy.copy(original)

clone["skills"].append("Heal")

print(original)
```

The nested `skills` list may also change because both objects refer to the same list.

---

## Deep Copy

```python
copy.deepcopy()
```

Copies the nested objects too.

```python
import copy

original = {
    "name": "Warrior",
    "skills": ["Attack", "Defend"]
}

clone = copy.deepcopy(original)

clone["skills"].append("Heal")

print(original)
print(clone)
```

Now the nested lists are independent.

---

# 💻 Real-World Project

## Project: Game Character Clone System

Build a game character creation system.

Create prototypes:

```text
Character Prototype
       │
       ├── Warrior
       ├── Mage
       ├── Archer
       └── Healer
```

Each prototype contains:

```text
Health
Weapon
Armor
Skills
Speed
Abilities
```

Instead of constructing every character from scratch:

```python
warrior_clone = warrior.clone()
```

Then customize:

```python
warrior_clone.weapon = "Axe"
warrior_clone.health = 120
```

### Learning outcome

You learn:

* object cloning
* shallow copy
* deep copy
* object identity
* efficient creation
* prototype-based object creation

---

# 🏆 Complete Comparison

| Pattern              | Main Problem                          | Solution                      | Real-World Example |
| -------------------- | ------------------------------------- | ----------------------------- | ------------------ |
| **Singleton**        | Too many instances                    | Allow only one instance       | Logger             |
| **Factory Method**   | Creation logic grows                  | Let subclasses decide product | Restaurant         |
| **Abstract Factory** | Creating related objects consistently | Create object families        | Cross-platform UI  |
| **Builder**          | Complex constructors                  | Build step-by-step            | Custom PC          |
| **Prototype**        | Repeated expensive creation           | Clone existing object         | Game characters    |

---

# 🧠 The Five Patterns Together

Imagine we are building a large game.

### Singleton

We need one central game configuration.

```text
GameConfig
    ↓
ONE instance
```

### Factory Method

We need to create different enemies.

```text
EnemyFactory
    ↓
Goblin / Zombie / Robot
```

### Abstract Factory

We need a complete themed set of game components.

```text
FantasyFactory
    ├── FantasyEnemy
    ├── FantasyWeapon
    └── FantasyEnvironment
```

### Builder

We need to create a customized character.

```text
CharacterBuilder
    ↓
Health
Weapon
Armor
Skills
```

### Prototype

We already have a configured enemy and need many similar enemies.

```text
Enemy Prototype
     ↓
Clone
     ↓
Clone
     ↓
Clone
```

---

# 🔥 Factory vs Builder vs Prototype

These three are commonly confused.

## Factory

> **Which object should I create?**

Example:

```text
PaymentFactory
      ↓
UPI / Card / PayPal
```

---

## Builder

> **How should I construct this object?**

Example:

```text
ComputerBuilder
      ↓
CPU
RAM
GPU
Storage
```

---

## Prototype

> **Can I copy an existing object?**

Example:

```text
Warrior Prototype
      ↓
Clone
      ↓
Clone
```

---

# 🔥 Factory Method vs Abstract Factory

Another important distinction.

### Factory Method

Usually focuses on creating a **single product type** through an overridable creation method.

```text
Restaurant
    ↓
create_food()
    ↓
Pizza / Burger
```

### Abstract Factory

Creates a **family of related products**.

```text
WindowsFactory
    ├── Button
    ├── Checkbox
    └── Menu
```

---

# 🔗 Creational Patterns and SOLID

Design Patterns and SOLID are different concepts, but they often work together.

| Pattern          | SOLID Connection                                     |
| ---------------- | ---------------------------------------------------- |
| Singleton        | Can create global-state/testing concerns if overused |
| Factory Method   | Strong connection to OCP                             |
| Abstract Factory | OCP + DIP                                            |
| Builder          | Helps separate construction responsibilities         |
| Prototype        | Reduces repeated construction logic                  |

---

# 🧪 Pattern Selection Guide

When you face an object-creation problem, ask:

### Question 1

> Do I need exactly one instance?

```text
YES → Singleton
```

### Question 2

> Does the subclass need to decide which object is created?

```text
YES → Factory Method
```

### Question 3

> Do I need a family of related objects?

```text
YES → Abstract Factory
```

### Question 4

> Does the object have many configuration steps/options?

```text
YES → Builder
```

### Question 5

> Do I already have a similar object that I can copy?

```text
YES → Prototype
```

---

# 🧠 Final Cheat Sheet

```text
                    CREATIONAL PATTERNS
                           │
       ┌───────────┬───────┼───────┬───────────┐
       │           │       │       │           │
   Singleton    Factory   Abstract Builder   Prototype
                 Method   Factory
       │           │        │        │           │
      ONE       WHICH?   FAMILY?    HOW?       COPY
       │           │        │        │           │
   One object   Product   Related  Steps     Existing
                choice    products           object
```

---

# 📌 One-Line Definitions for Revision

### Singleton

> Ensures that only one instance of a class exists and provides access to it.

### Factory Method

> Defines an object-creation method while allowing subclasses to decide which concrete product to create.

### Abstract Factory

> Provides an interface for creating families of related objects without specifying their concrete classes.

### Builder

> Separates complex object construction from its representation and allows step-by-step construction.

### Prototype

> Creates new objects by copying an existing object instead of constructing them from scratch.

---

# 🚀 Suggested GitHub Projects

To master the patterns through implementation, build these five projects:

| Pattern          | Project                        | Main Concepts                                |
| ---------------- | ------------------------------ | -------------------------------------------- |
| Singleton        | 📝 Application Logger          | One shared instance, `__new__`, shared state |
| Factory Method   | 💳 Payment Processing System   | Polymorphism, OCP                            |
| Abstract Factory | 🖥️ Cross-Platform UI System   | Product families, DIP                        |
| Builder          | 🖥️ Custom PC Builder          | Step-by-step construction, chaining          |
| Prototype        | 🎮 Game Character Clone System | Cloning, shallow/deep copy                   |

---

# 🎯 Recommended Folder Structure

Create one GitHub repository:

```text
design-patterns-python/
│
├── README.md
│
└── creational/
    │
    ├── singleton/
    │   ├── README.md
    │   └── logger.py
    │
    ├── factory_method/
    │   ├── README.md
    │   └── payment_system.py
    │
    ├── abstract_factory/
    │   ├── README.md
    │   └── cross_platform_ui.py
    │
    ├── builder/
    │   ├── README.md
    │   └── pc_builder.py
    │
    └── prototype/
        ├── README.md
        └── character_clone.py
```

---

# 🏁 Final Mental Model

Whenever you see an object-creation problem, remember:

```text
"Only ONE?"
      ↓
  Singleton

"WHICH object?"
      ↓
 Factory Method

"WHICH FAMILY?"
      ↓
Abstract Factory

"HOW do I build it?"
      ↓
   Builder

"Can I COPY one?"
      ↓
  Prototype
```

---

# 📚 Learning Status

## Creational Patterns

* [x] Singleton
* [x] Factory Method
* [x] Abstract Factory
* [x] Builder
* [x] Prototype

## Structural Patterns

* [ ] Adapter
* [ ] Bridge
* [ ] Composite
* [ ] Decorator
* [ ] Facade
* [ ] Flyweight
* [ ] Proxy

## Behavioral Patterns

* [ ] Chain of Responsibility
* [ ] Command
* [ ] Interpreter
* [ ] Iterator
* [ ] Mediator
* [ ] Memento
* [ ] Observer
* [ ] State
* [ ] Strategy
* [ ] Template Method
* [ ] Visitor

---

# ⭐ Key Takeaway

> **Creational Design Patterns are about controlling and improving how objects are created.**

Remember:

```text
Singleton        → ONE
Factory Method   → WHICH?
Abstract Factory → FAMILY?
Builder          → HOW?
Prototype        → COPY?
```
