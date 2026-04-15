# Part 6: UML & Problem Recognition - Complete Guide

## Table of Contents
1. [UML Basics](#uml-basics)
2. [Class Diagrams](#class-diagrams)
3. [Relationships](#relationships)
4. [Pattern-Specific UML](#pattern-specific-uml)
5. [Problem Recognition Guide](#problem-recognition-guide)
6. [Interview Question Patterns](#interview-question-patterns)

---

# UML Basics

## What is UML?

**Unified Modeling Language (UML)** is a standardized way to visualize software design.

### Why Learn UML?
- ✅ Communicate design clearly
- ✅ Document architecture
- ✅ Plan before coding
- ✅ Common language for developers

---

# Class Diagrams

## Basic Class Representation

```
┌─────────────────────┐
│     ClassName       │  ← Class name
├─────────────────────┤
│ - privateField      │  ← Attributes (fields)
│ + publicField       │
│ # protectedField    │
│ ~ packageField      │
├─────────────────────┤
│ + publicMethod()    │  ← Methods
│ - privateMethod()   │
│ # protectedMethod() │
└─────────────────────┘
```

### Visibility Symbols
- `+` Public
- `-` Private
- `#` Protected
- `~` Package (default)

### Example: Person Class

```
┌─────────────────────┐
│       Person        │
├─────────────────────┤
│ - name: String      │
│ - age: int          │
│ - email: String     │
├─────────────────────┤
│ + Person(name, age) │
│ + getName(): String │
│ + getAge(): int     │
│ + setEmail(email)   │
└─────────────────────┘
```

**Java Code:**
```java
class Person {
    private String name;
    private int age;
    private String email;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public String getName() { return name; }
    public int getAge() { return age; }
    public void setEmail(String email) { this.email = email; }
}
```

---

# Relationships

## 1. Association (Uses-A)

**One class uses another class.**

```
┌──────────┐              ┌──────────┐
│ Student  │─────────────>│ Course   │
└──────────┘              └──────────┘
   "enrolls in"
```

**Java Code:**
```java
class Student {
    public void enroll(Course course) {
        // Student uses Course
    }
}

class Course {
    private String name;
}
```

### Multiplicity

```
┌──────────┐  1      *   ┌──────────┐
│ Teacher  │─────────────│ Student  │
└──────────┘             └──────────┘
   "teaches"

1 = exactly one
* = zero or more
0..1 = zero or one
1..* = one or more
2..5 = two to five
```

**Java Code:**
```java
class Teacher {
    private List<Student> students;  // One teacher, many students
}
```

## 2. Aggregation (Has-A, Weak)

**Whole-part relationship where parts can exist independently.**

```
┌──────────┐              ┌──────────┐
│Department│◇────────────>│ Employee │
└──────────┘              └──────────┘
   "has"
   
◇ = hollow diamond (aggregation)
```

**Java Code:**
```java
class Department {
    private List<Employee> employees;
    
    public void addEmployee(Employee emp) {
        employees.add(emp);
    }
}

class Employee {
    private String name;
}

// Employee can exist without Department
Employee emp = new Employee("John");
```

## 3. Composition (Has-A, Strong)

**Whole-part relationship where parts cannot exist without the whole.**

```
┌──────────┐              ┌──────────┐
│  House   │◆────────────>│  Room    │
└──────────┘              └──────────┘
   "contains"
   
◆ = filled diamond (composition)
```

**Java Code:**
```java
class House {
    private List<Room> rooms;
    
    public House() {
        rooms = new ArrayList<>();
        rooms.add(new Room("Living Room"));
        rooms.add(new Room("Bedroom"));
    }
}

class Room {
    private String name;
    
    public Room(String name) {
        this.name = name;
    }
}

// Room cannot exist without House
// When House is destroyed, Rooms are destroyed too
```

## 4. Inheritance (Is-A)

**Child class inherits from parent class.**

```
┌──────────┐
│  Animal  │
└────△─────┘
     │
     │ (inheritance)
     │
┌────┴─────┬──────────┐
│   Dog    │   Cat    │
└──────────┴──────────┘

△ = hollow triangle pointing to parent
```

**Java Code:**
```java
class Animal {
    public void eat() { }
}

class Dog extends Animal {
    public void bark() { }
}

class Cat extends Animal {
    public void meow() { }
}
```

## 5. Realization (Implements)

**Class implements an interface.**

```
┌──────────────┐
│  «interface» │
│   Flyable    │
└──────△───────┘
       │
       │ (realization)
       │ - - - - (dashed line)
       │
┌──────┴───────┐
│     Bird     │
└──────────────┘

△ with dashed line = realization
```

**Java Code:**
```java
interface Flyable {
    void fly();
}

class Bird implements Flyable {
    @Override
    public void fly() {
        System.out.println("Flying");
    }
}
```

## 6. Dependency (Uses)

**One class depends on another temporarily.**

```
┌──────────┐              ┌──────────┐
│OrderProc │- - - - - - ->│ Payment  │
└──────────┘              └──────────┘
   "uses"
   
- - - -> = dashed arrow (dependency)
```

**Java Code:**
```java
class OrderProcessor {
    public void process(Order order, Payment payment) {
        // Uses Payment temporarily
        payment.charge(order.getTotal());
    }
}

class Payment {
    public void charge(double amount) { }
}
```

---

# Pattern-Specific UML

## Singleton Pattern

```
┌─────────────────────────────┐
│      DatabaseConnection     │
├─────────────────────────────┤
│ - instance: DatabaseConn    │ ← Static instance
├─────────────────────────────┤
│ - DatabaseConnection()      │ ← Private constructor
│ + getInstance(): DatabaseC  │ ← Static method
│ + query(sql): ResultSet     │
└─────────────────────────────┘
```

## Factory Pattern

```
┌──────────────┐
│ «interface»  │
│    Shape     │
└──────△───────┘
       │
       │ - - - - (implements)
       │
┌──────┴───────┬──────────┬──────────┐
│   Circle     │Rectangle │ Triangle │
└──────────────┴──────────┴──────────┘
       ↑
       │ (creates)
       │
┌──────┴───────┐
│ShapeFactory  │
├──────────────┤
│+createShape()│
└──────────────┘
```

## Strategy Pattern

```
┌──────────────┐              ┌──────────────┐
│   Context    │◇────────────>│ «interface»  │
│              │              │  Strategy    │
└──────────────┘              └──────△───────┘
                                     │
                                     │
                    ┌────────────────┼────────────────┐
                    │                │                │
              ┌─────┴─────┐   ┌─────┴─────┐   ┌─────┴─────┐
              │StrategyA  │   │StrategyB  │   │StrategyC  │
              └───────────┘   └───────────┘   └───────────┘
```

## Observer Pattern

```
┌──────────────┐              ┌──────────────┐
│   Subject    │◇────────────>│ «interface»  │
│              │              │  Observer    │
│ +attach()    │              │              │
│ +detach()    │              │ +update()    │
│ +notify()    │              └──────△───────┘
└──────────────┘                     │
                                     │
                    ┌────────────────┼────────────────┐
                    │                │                │
              ┌─────┴─────┐   ┌─────┴─────┐   ┌─────┴─────┐
              │ObserverA  │   │ObserverB  │   │ObserverC  │
              └───────────┘   └───────────┘   └───────────┘
```

## Decorator Pattern

```
┌──────────────┐
│ «interface»  │
│  Component   │
└──────△───────┘
       │
       │
┌──────┴───────┬──────────────────┐
│              │                  │
│ Concrete     │    Decorator     │
│ Component    │    (abstract)    │
│              │                  │
└──────────────┘  └──────△────────┘
                         │
                         │
                  ┌──────┴───────┬──────────────┐
                  │              │              │
            DecoratorA     DecoratorB     DecoratorC
```

---

# Problem Recognition Guide

## Pattern Recognition Flowchart

```
START: What's the problem?
│
├─> Need exactly ONE instance?
│   └─> Use SINGLETON
│
├─> Too many subclasses?
│   └─> Use BRIDGE (separate hierarchies)
│
├─> Too many if-else statements?
│   └─> Use STRATEGY (polymorphism)
│
├─> Complex object construction?
│   └─> Use BUILDER (step-by-step)
│
├─> Interface mismatch?
│   └─> Use ADAPTER (wrapper)
│
├─> Add functionality dynamically?
│   └─> Use DECORATOR (wrap objects)
│
├─> Simplify complex system?
│   └─> Use FACADE (simple interface)
│
├─> Notify multiple objects?
│   └─> Use OBSERVER (publish-subscribe)
│
├─> Undo/redo functionality?
│   └─> Use COMMAND (encapsulate requests)
│
└─> Create families of objects?
    └─> Use ABSTRACT FACTORY
```

## Problem → Solution Mapping

### Problem: "Global state issue"

**Symptoms:**
- Need single instance
- Shared resource (DB connection, config)
- Global access point

**Solution:** Singleton (but consider Dependency Injection instead!)

```java
// ❌ Singleton (global state)
DatabaseConnection.getInstance().query("...");

// ✅ Better: Dependency Injection
class UserService {
    private DatabaseConnection db;
    
    public UserService(DatabaseConnection db) {
        this.db = db;
    }
}
```

### Problem: "Too many subclasses"

**Symptoms:**
```
RedCircle, BlueCircle, GreenCircle
RedSquare, BlueSquare, GreenSquare
RedTriangle, BlueTriangle, GreenTriangle
...
```

**Solution:** Bridge Pattern

```
Shape (abstraction) + Color (implementation)
Circle, Square, Triangle × Red, Blue, Green
= 3 + 3 = 6 classes instead of 9!
```

### Problem: "Too many if-else"

**Symptoms:**
```java
if (paymentType == "CREDIT_CARD") {
    // ...
} else if (paymentType == "PAYPAL") {
    // ...
} else if (paymentType == "BITCOIN") {
    // ...
}
```

**Solution:** Strategy Pattern

```java
interface PaymentStrategy {
    void pay(double amount);
}

class CreditCardStrategy implements PaymentStrategy { }
class PayPalStrategy implements PaymentStrategy { }
class BitcoinStrategy implements PaymentStrategy { }
```

### Problem: "Memory leak"

**Common Causes:**

1. **Static collections**
```java
// ❌ Leak
private static List<User> cache = new ArrayList<>();
```

2. **Unclosed resources**
```java
// ❌ Leak
FileInputStream fis = new FileInputStream(file);
// Never closed!
```

3. **Listeners not removed**
```java
// ❌ Leak
eventSource.addListener(listener);
// Never removed!
```

4. **ThreadLocal not cleared**
```java
// ❌ Leak
ThreadLocal<User> user = new ThreadLocal<>();
user.set(currentUser);
// Never removed!
```

**Solutions:**
- Use bounded caches
- Use try-with-resources
- Always remove listeners
- Clear ThreadLocal in finally block

---

# Interview Question Patterns

## Pattern 1: Design a System

**Question:** "Design a parking lot system"

**Approach:**
1. **Identify entities** (nouns)
   - ParkingLot, ParkingSpot, Vehicle, Ticket

2. **Identify relationships**
   - ParkingLot HAS-MANY ParkingSpots
   - Vehicle PARKS-IN ParkingSpot
   - Ticket ISSUED-FOR Vehicle

3. **Apply patterns**
   - Factory for creating different vehicle types
   - Strategy for pricing
   - Singleton for ParkingLot

```java
class ParkingLot {
    private static ParkingLot instance;
    private List<ParkingSpot> spots;
    
    private ParkingLot() { }
    
    public static ParkingLot getInstance() {
        if (instance == null) {
            instance = new ParkingLot();
        }
        return instance;
    }
}

interface Vehicle {
    VehicleType getType();
}

class Car implements Vehicle { }
class Motorcycle implements Vehicle { }

interface PricingStrategy {
    double calculatePrice(int hours);
}

class HourlyPricing implements PricingStrategy { }
class DailyPricing implements PricingStrategy { }
```

## Pattern 2: Identify the Pattern

**Question:** "What pattern is this?"

```java
class Coffee {
    private String description;
    private double cost;
}

class MilkDecorator extends Coffee {
    private Coffee coffee;
    
    public MilkDecorator(Coffee coffee) {
        this.coffee = coffee;
    }
}
```

**Answer:** Decorator Pattern
- Wraps another object
- Adds functionality dynamically
- Same interface as wrapped object

## Pattern 3: Fix the Code

**Question:** "What's wrong with this code?"

```java
class Singleton {
    private static Singleton instance;
    
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

**Answer:** Not thread-safe!

**Fix:**
```java
class Singleton {
    private static volatile Singleton instance;
    
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

## Pattern 4: Choose the Right Pattern

**Scenario:** "You need to create different types of documents (PDF, Word, Excel) but the creation logic is complex."

**Options:**
- A) Singleton
- B) Factory Method
- C) Observer
- D) Strategy

**Answer:** B) Factory Method
- Creates objects without specifying exact class
- Encapsulates creation logic
- Easy to add new document types

## Pattern 5: SOLID Violation

**Question:** "Which SOLID principle is violated?"

```java
class Employee {
    public void calculateSalary() { }
    public void saveToDatabase() { }
    public void generateReport() { }
}
```

**Answer:** Single Responsibility Principle (SRP)
- Class has multiple responsibilities
- Should be split into separate classes

**Fix:**
```java
class Employee {
    // Just data
}

class SalaryCalculator {
    public void calculate(Employee emp) { }
}

class EmployeeRepository {
    public void save(Employee emp) { }
}

class ReportGenerator {
    public void generate(Employee emp) { }
}
```

---

# Quick Reference Tables

## When to Use Which Pattern

| Problem | Pattern | Example |
|---------|---------|---------|
| One instance needed | Singleton | Database connection |
| Complex construction | Builder | Pizza with many toppings |
| Create without specifying class | Factory | Shape factory |
| Interface mismatch | Adapter | USB to USB-C |
| Separate abstraction/implementation | Bridge | Shape + Color |
| Add functionality dynamically | Decorator | Coffee + Milk + Sugar |
| Simplify complex system | Facade | Computer startup |
| Interchangeable algorithms | Strategy | Payment methods |
| Notify dependents | Observer | YouTube subscriptions |
| Undo/redo | Command | Text editor |

## SOLID Quick Check

| Principle | Question | Good Sign |
|-----------|----------|-----------|
| **SRP** | How many reasons to change? | One |
| **OCP** | Can I add features without modifying? | Yes |
| **LSP** | Can I substitute subclass? | Yes |
| **ISP** | Are all interface methods used? | Yes |
| **DIP** | Do I depend on abstractions? | Yes |

## Memory Leak Checklist

| Cause | Check | Fix |
|-------|-------|-----|
| Static collections | Growing forever? | Bounded cache |
| Resources | Closed? | try-with-resources |
| Listeners | Removed? | removeListener() |
| ThreadLocal | Cleared? | remove() in finally |
| Inner classes | Static? | Use static inner class |

---

## Summary

### UML Key Points

1. **Class Diagrams**
   - Show structure of classes
   - Attributes and methods
   - Visibility modifiers

2. **Relationships**
   - Association (uses)
   - Aggregation (has, weak)
   - Composition (has, strong)
   - Inheritance (is-a)
   - Realization (implements)
   - Dependency (temporary use)

3. **Pattern UML**
   - Each pattern has characteristic structure
   - Helps identify patterns in code

### Problem Recognition

1. **Identify Symptoms**
   - Too many subclasses → Bridge
   - Too many if-else → Strategy
   - Global state → Singleton/DI
   - Interface mismatch → Adapter

2. **Apply Solution**
   - Choose appropriate pattern
   - Follow SOLID principles
   - Consider trade-offs

3. **Verify**
   - Does it solve the problem?
   - Is it maintainable?
   - Is it testable?

### Interview Tips

✅ **DO:**
- Think out loud
- Ask clarifying questions
- Start with simple solution
- Discuss trade-offs
- Draw diagrams

❌ **DON'T:**
- Jump to complex solution
- Ignore requirements
- Forget edge cases
- Over-engineer
- Memorize without understanding

### Final Advice

**Practice recognizing patterns in real code:**
- Open source projects
- Framework source code (Spring, Android)
- Your own projects

**Remember:**
- Patterns are tools, not rules
- Understand the problem first
- Simple solutions are often best
- Good design evolves over time

**Keep learning and practicing!** 🚀

---

## Complete Learning Path

You've now covered:

1. ✅ **Core OOP Principles** - Foundation
2. ✅ **SOLID Principles** - Design guidelines
3. ✅ **Design Patterns** - Reusable solutions
4. ✅ **Java Memory & Internals** - How it works
5. ✅ **Multithreading & Architecture** - Concurrent & scalable design
6. ✅ **UML & Problem Recognition** - Visualization & application

**Next Steps:**
- Practice coding these patterns
- Read open source code
- Build projects using these concepts
- Teach others (best way to learn!)

**Good luck with your interviews and projects!** 💪