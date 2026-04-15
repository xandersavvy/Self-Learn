# Part 1: Core OOP Principles - Complete Guide

## Table of Contents
1. [Encapsulation](#encapsulation)
2. [Inheritance](#inheritance)
3. [Polymorphism](#polymorphism)
4. [Abstraction](#abstraction)
5. [Access Modifiers](#access-modifiers)

---

# 1. Encapsulation (Data Hiding & Immutability)

## What is Encapsulation?
Encapsulation means **bundling data (variables) and methods that operate on that data within a single unit (class)**, and **restricting direct access** to some of the object's components.

Think of it like a **capsule** - the medicine (data) is protected inside, and you can only access it through the proper way (methods).

## Why Do We Need It?
- **Control**: You control how data is accessed and modified
- **Validation**: You can validate data before setting it
- **Flexibility**: You can change internal implementation without affecting external code
- **Security**: Hide sensitive data from outside world

## ❌ Bad Example (No Encapsulation)
```java
class BankAccount {
    public double balance;  // Anyone can modify this directly!
    public String accountNumber;
}

// Usage - DANGEROUS!
BankAccount account = new BankAccount();
account.balance = 1000000;  // Direct access - no validation!
account.balance = -500;     // Negative balance allowed! BUG!
account.accountNumber = "HACKED";  // Can be changed by anyone!
```

**Problems:**
- No validation - can set negative balance
- No control - anyone can modify anything
- No business rules enforcement
- Hard to track who changed what

## ✅ Good Example (With Encapsulation)
```java
class BankAccount {
    private double balance;  // Hidden from outside
    private final String accountNumber;  // Immutable after creation
    
    public BankAccount(String accountNumber) {
        this.accountNumber = accountNumber;
        this.balance = 0.0;
    }
    
    // Controlled access through methods
    public void deposit(double amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Deposit amount must be positive");
        }
        balance += amount;
        System.out.println("Deposited: $" + amount);
    }
    
    public void withdraw(double amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Withdrawal amount must be positive");
        }
        if (amount > balance) {
            throw new IllegalArgumentException("Insufficient funds");
        }
        balance -= amount;
        System.out.println("Withdrawn: $" + amount);
    }
    
    // Read-only access
    public double getBalance() {
        return balance;
    }
    
    public String getAccountNumber() {
        return accountNumber;
    }
}

// Usage - SAFE!
BankAccount account = new BankAccount("ACC-12345");
account.deposit(1000);
account.withdraw(200);
System.out.println("Balance: $" + account.getBalance());

// account.balance = -500;  // Compilation error - can't access private field
// account.accountNumber = "HACKED";  // Compilation error - final field
```

## Immutability

**Immutable objects** cannot be changed after creation. This makes them:
- Thread-safe (no synchronization needed)
- Easier to reason about
- Safe to share across code
- Cacheable

### Example: Immutable Person Class
```java
public final class Person {  // final = cannot be extended
    private final String name;
    private final int age;
    private final String email;
    
    public Person(String name, int age, String email) {
        this.name = name;
        this.age = age;
        this.email = email;
    }
    
    // Only getters, no setters
    public String getName() { return name; }
    public int getAge() { return age; }
    public String getEmail() { return email; }
    
    // To "modify", create a new object
    public Person withAge(int newAge) {
        return new Person(this.name, newAge, this.email);
    }
    
    public Person withEmail(String newEmail) {
        return new Person(this.name, this.age, newEmail);
    }
}

// Usage
Person john = new Person("John", 25, "john@example.com");
Person olderJohn = john.withAge(26);  // Creates new object

System.out.println(john.getAge());      // 25 - original unchanged
System.out.println(olderJohn.getAge()); // 26 - new object
```

### Real-World Example: String is Immutable
```java
String str1 = "Hello";
String str2 = str1.toUpperCase();  // Creates NEW string

System.out.println(str1);  // "Hello" - unchanged
System.out.println(str2);  // "HELLO" - new string
```

---

# 2. Inheritance (IS-A Relationship)

## What is Inheritance?
Inheritance allows a class to **inherit properties and methods from another class**. It represents an **"IS-A"** relationship.

## Real-World Analogy
- A Dog **IS-A** Animal
- A Car **IS-A** Vehicle
- A Manager **IS-A** Employee
- A SavingsAccount **IS-A** BankAccount

## Basic Example: Animal Hierarchy
```java
// Parent class (Superclass/Base class)
class Animal {
    protected String name;  // protected = accessible to subclasses
    protected int age;
    
    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
        System.out.println("Animal constructor called");
    }
    
    public void eat() {
        System.out.println(name + " is eating");
    }
    
    public void sleep() {
        System.out.println(name + " is sleeping");
    }
    
    public void makeSound() {
        System.out.println(name + " makes a sound");
    }
}

// Child class (Subclass/Derived class)
class Dog extends Animal {
    private String breed;
    
    public Dog(String name, int age, String breed) {
        super(name, age);  // Call parent constructor - MUST be first line
        this.breed = breed;
        System.out.println("Dog constructor called");
    }
    
    // Dog has all Animal methods PLUS its own
    public void bark() {
        System.out.println(name + " says: Woof! Woof!");
    }
    
    // Override parent method
    @Override
    public void makeSound() {
        bark();  // Dog-specific implementation
    }
    
    public void fetch() {
        System.out.println(name + " is fetching the ball");
    }
}

class Cat extends Animal {
    private boolean isIndoor;
    
    public Cat(String name, int age, boolean isIndoor) {
        super(name, age);
        this.isIndoor = isIndoor;
    }
    
    public void meow() {
        System.out.println(name + " says: Meow!");
    }
    
    @Override
    public void makeSound() {
        meow();
    }
    
    public void scratch() {
        System.out.println(name + " is scratching");
    }
}

// Usage
Dog dog = new Dog("Buddy", 3, "Golden Retriever");
// Output:
// Animal constructor called
// Dog constructor called

dog.eat();        // Inherited from Animal
dog.sleep();      // Inherited from Animal
dog.bark();       // Dog's own method
dog.fetch();      // Dog's own method
dog.makeSound();  // Overridden method

Cat cat = new Cat("Whiskers", 2, true);
cat.eat();        // Inherited from Animal
cat.meow();       // Cat's own method
cat.scratch();    // Cat's own method
```

## Constructor Chaining: super() vs this()

### super() - Call Parent Constructor
```java
class Vehicle {
    protected String brand;
    protected int year;
    
    public Vehicle(String brand, int year) {
        this.brand = brand;
        this.year = year;
        System.out.println("Vehicle constructor: " + brand);
    }
}

class Car extends Vehicle {
    private int doors;
    private String fuelType;
    
    public Car(String brand, int year, int doors, String fuelType) {
        super(brand, year);  // MUST be first line - calls Vehicle constructor
        this.doors = doors;
        this.fuelType = fuelType;
        System.out.println("Car constructor: " + doors + " doors");
    }
}

class ElectricCar extends Car {
    private int batteryCapacity;
    
    public ElectricCar(String brand, int year, int doors, int batteryCapacity) {
        super(brand, year, doors, "Electric");  // Calls Car constructor
        this.batteryCapacity = batteryCapacity;
        System.out.println("ElectricCar constructor: " + batteryCapacity + "kWh");
    }
}

// Usage
ElectricCar tesla = new ElectricCar("Tesla", 2024, 4, 100);
// Output:
// Vehicle constructor: Tesla
// Car constructor: 4 doors
// ElectricCar constructor: 100kWh
```

### this() - Call Another Constructor in Same Class
```java
class Rectangle {
    private int width;
    private int height;
    
    // Constructor 1: Full parameters
    public Rectangle(int width, int height) {
        this.width = width;
        this.height = height;
        System.out.println("Rectangle created: " + width + "x" + height);
    }
    
    // Constructor 2: Square (equal sides)
    public Rectangle(int side) {
        this(side, side);  // Calls Constructor 1
        System.out.println("Square created");
    }
    
    // Constructor 3: Default (unit square)
    public Rectangle() {
        this(1);  // Calls Constructor 2, which calls Constructor 1
        System.out.println("Default rectangle created");
    }
    
    public int getArea() {
        return width * height;
    }
}

// Usage
Rectangle rect1 = new Rectangle(5, 10);
// Output: Rectangle created: 5x10

Rectangle square = new Rectangle(5);
// Output: 
// Rectangle created: 5x5
// Square created

Rectangle defaultRect = new Rectangle();
// Output:
// Rectangle created: 1x1
// Square created
// Default rectangle created
```

### Important Constructor Rules
1. **super() or this() must be the FIRST statement** in a constructor
2. You **cannot use both** super() and this() in the same constructor
3. If you don't call super(), Java automatically calls super() (no-arg parent constructor)
4. If parent has no no-arg constructor, you MUST explicitly call super(args)

```java
class Parent {
    public Parent(String name) {  // No no-arg constructor
        System.out.println("Parent: " + name);
    }
}

class Child extends Parent {
    public Child() {
        // super();  // ERROR: Parent() doesn't exist
        super("Default");  // MUST provide argument
    }
}
```

---

# 3. Polymorphism

## What is Polymorphism?
**Polymorphism** means "many forms". It allows objects of different types to be treated through the same interface.

**Real-world analogy:** A person can be a student, employee, parent at the same time - same person, different roles.

## Two Types of Polymorphism

### 1. Compile-Time Polymorphism (Method Overloading)
**Same method name, different parameters** in the same class. Decided at **compile time**.

```java
class Calculator {
    // Method 1: Add two integers
    public int add(int a, int b) {
        System.out.println("Adding two integers");
        return a + b;
    }
    
    // Method 2: Add three integers
    public int add(int a, int b, int c) {
        System.out.println("Adding three integers");
        return a + b + c;
    }
    
    // Method 3: Add two doubles
    public double add(double a, double b) {
        System.out.println("Adding two doubles");
        return a + b;
    }
    
    // Method 4: Add array of integers
    public int add(int[] numbers) {
        System.out.println("Adding array of integers");
        int sum = 0;
        for (int num : numbers) {
            sum += num;
        }
        return sum;
    }
    
    // Method 5: Concatenate strings (different type)
    public String add(String a, String b) {
        System.out.println("Concatenating strings");
        return a + b;
    }
}

// Usage
Calculator calc = new Calculator();

System.out.println(calc.add(5, 10));              // Calls Method 1 → 15
System.out.println(calc.add(5, 10, 15));          // Calls Method 2 → 30
System.out.println(calc.add(5.5, 10.5));          // Calls Method 3 → 16.0
System.out.println(calc.add(new int[]{1,2,3,4})); // Calls Method 4 → 10
System.out.println(calc.add("Hello", "World"));   // Calls Method 5 → HelloWorld
```

**Rules for Method Overloading:**
- ✅ Different number of parameters
- ✅ Different types of parameters
- ✅ Different order of parameters
- ❌ Return type alone is NOT enough!
- ❌ Access modifier alone is NOT enough!

```java
// VALID overloading
class Valid {
    public void method(int a) { }
    public void method(double a) { }           // Different type ✓
    public void method(int a, int b) { }       // Different number ✓
    public void method(double a, int b) { }    // Different order ✓
}

// INVALID overloading
class Invalid {
    public int getValue() { return 1; }
    public double getValue() { return 1.0; }   // ERROR: Same signature!
    
    public void process(int a) { }
    private void process(int a) { }            // ERROR: Same signature!
}
```

### 2. Runtime Polymorphism (Method Overriding)
**Child class provides specific implementation** of a method already defined in parent class. Decided at **runtime**.

```java
class Shape {
    protected String color;
    
    public Shape(String color) {
        this.color = color;
    }
    
    public void draw() {
        System.out.println("Drawing a shape");
    }
    
    public double area() {
        return 0;
    }
    
    public void displayInfo() {
        System.out.println("Shape color: " + color);
    }
}

class Circle extends Shape {
    private double radius;
    
    public Circle(String color, double radius) {
        super(color);
        this.radius = radius;
    }
    
    @Override  // Good practice - compiler checks if you're actually overriding
    public void draw() {
        System.out.println("Drawing a " + color + " circle");
    }
    
    @Override
    public double area() {
        return Math.PI * radius * radius;
    }
}

class Rectangle extends Shape {
    private double width, height;
    
    public Rectangle(String color, double width, double height) {
        super(color);
        this.width = width;
        this.height = height;
    }
    
    @Override
    public void draw() {
        System.out.println("Drawing a " + color + " rectangle");
    }
    
    @Override
    public double area() {
        return width * height;
    }
}

class Triangle extends Shape {
    private double base, height;
    
    public Triangle(String color, double base, double height) {
        super(color);
        this.base = base;
        this.height = height;
    }
    
    @Override
    public void draw() {
        System.out.println("Drawing a " + color + " triangle");
    }
    
    @Override
    public double area() {
        return 0.5 * base * height;
    }
}

// THE MAGIC OF POLYMORPHISM
Shape shape1 = new Circle("Red", 5);        // Parent reference, child object
Shape shape2 = new Rectangle("Blue", 4, 6);
Shape shape3 = new Triangle("Green", 3, 4);

// Same code works for all shapes!
shape1.draw();  // "Drawing a Red circle" - calls Circle's method
shape2.draw();  // "Drawing a Blue rectangle" - calls Rectangle's method
shape3.draw();  // "Drawing a Green triangle" - calls Triangle's method

System.out.println("Circle area: " + shape1.area());     // 78.54
System.out.println("Rectangle area: " + shape2.area());  // 24.0
System.out.println("Triangle area: " + shape3.area());   // 6.0
```

### Dynamic Dispatch (Runtime Method Resolution)
The JVM decides **at runtime** which method to call based on the **actual object type**, not the reference type.

```java
Shape[] shapes = {
    new Circle("Red", 5),
    new Rectangle("Blue", 4, 6),
    new Triangle("Green", 3, 4),
    new Circle("Yellow", 3)
};

// Same code works for all shapes!
double totalArea = 0;
for (Shape shape : shapes) {
    shape.draw();   // Calls the correct method for each object
    double area = shape.area();
    System.out.println("Area: " + area);
    totalArea += area;
}
System.out.println("Total area: " + totalArea);
```

### Method Overriding Rules (VERY IMPORTANT!)

```java
class Parent {
    // Original method
    protected Number getValue() throws IOException {
        return 10;
    }
    
    public void display() {
        System.out.println("Parent display");
    }
}

class Child extends Parent {
    // ✅ Valid overriding
    @Override
    public Integer getValue() throws FileNotFoundException {
        return 20;
    }
    
    @Override
    public void display() {
        System.out.println("Child display");
    }
}
```

**The 7 Golden Rules of Method Overriding:**

1. **Method Signature**: Must be EXACTLY the same (name + parameters)
   ```java
   // Parent
   public void process(int a) { }
   
   // Child - VALID
   @Override
   public void process(int a) { }
   
   // Child - INVALID (different signature)
   public void process(double a) { }  // This is overloading, not overriding!
   ```

2. **Access Modifier**: Can be same or MORE permissive
   ```java
   // Parent: protected
   protected void method() { }
   
   // Child - VALID
   @Override
   public void method() { }  // protected → public ✓
   
   // Child - INVALID
   @Override
   private void method() { }  // protected → private ✗
   ```

3. **Return Type**: Can be same or COVARIANT (subtype)
   ```java
   // Parent
   public Number getValue() { return 10; }
   
   // Child - VALID (Integer is subclass of Number)
   @Override
   public Integer getValue() { return 20; }
   
   // Child - INVALID
   @Override
   public Object getValue() { return 20; }  // Object is superclass ✗
   ```

4. **Exceptions**: Can throw same, subclass, or no exception
   ```java
   // Parent
   public void method() throws IOException { }
   
   // Child - VALID
   @Override
   public void method() throws FileNotFoundException { }  // Subclass ✓
   
   @Override
   public void method() { }  // No exception ✓
   
   // Child - INVALID
   @Override
   public void method() throws Exception { }  // Superclass ✗
   ```

5. **static methods**: Cannot be overridden (they're hidden, not overridden)
   ```java
   class Parent {
       public static void staticMethod() {
           System.out.println("Parent static");
       }
   }
   
   class Child extends Parent {
       // This is METHOD HIDING, not overriding
       public static void staticMethod() {
           System.out.println("Child static");
       }
   }
   
   Parent p = new Child();
   p.staticMethod();  // "Parent static" - based on reference type, not object type
   ```

6. **final methods**: Cannot be overridden
   ```java
   class Parent {
       public final void finalMethod() {
           System.out.println("Cannot override this");
       }
   }
   
   class Child extends Parent {
       // @Override
       // public void finalMethod() { }  // ERROR: Cannot override final method
   }
   ```

7. **private methods**: Cannot be overridden (not inherited)
   ```java
   class Parent {
       private void privateMethod() {
           System.out.println("Private method");
       }
   }
   
   class Child extends Parent {
       // This is a NEW method, not overriding
       private void privateMethod() {
           System.out.println("Child's own method");
       }
   }
   ```

---

# 4. Abstraction (Interfaces vs Abstract Classes)

## What is Abstraction?
**Hiding complex implementation details** and showing only essential features.

**Real-World Analogy:**
- You drive a car using steering wheel, pedals, gear shift
- You don't need to know how the engine, transmission, or fuel injection works
- The complex details are **abstracted away**
- You interact with a simple interface

## Abstract Classes

**Abstract class** = A class that:
- Cannot be instantiated directly
- May contain abstract methods (methods without implementation)
- May contain concrete methods (methods with implementation)
- Can have constructors, fields, and any access modifiers

```java
abstract class Vehicle {
    protected String brand;
    protected int year;
    protected double fuelLevel;
    
    // Constructor
    public Vehicle(String brand, int year) {
        this.brand = brand;
        this.year = year;
        this.fuelLevel = 0;
    }
    
    // Concrete method (has implementation)
    public void displayInfo() {
        System.out.println(year + " " + brand);
        System.out.println("Fuel level: " + fuelLevel + "%");
    }
    
    public void refuel(double amount) {
        fuelLevel = Math.min(100, fuelLevel + amount);
        System.out.println("Refueled. Current level: " + fuelLevel + "%");
    }
    
    // Abstract methods (no implementation - subclasses MUST implement)
    public abstract void start();
    public abstract void stop();
    public abstract double calculateFuelEfficiency();
    public abstract int getMaxSpeed();
}

class Car extends Vehicle {
    private int doors;
    private String transmission;
    
    public Car(String brand, int year, int doors, String transmission) {
        super(brand, year);
        this.doors = doors;
        this.transmission = transmission;
    }
    
    // MUST implement all abstract methods
    @Override
    public void start() {
        System.out.println("Car starting: Turn key, engine roars to life");
    }
    
    @Override
    public void stop() {
        System.out.println("Car stopping: Press brake, turn off engine");
    }
    
    @Override
    public double calculateFuelEfficiency() {
        return 15.5;  // km per liter
    }
    
    @Override
    public int getMaxSpeed() {
        return 180;  // km/h
    }
    
    public void openTrunk() {
        System.out.println("Trunk opened");
    }
}

class Motorcycle extends Vehicle {
    private boolean hasSidecar;
    
    public Motorcycle(String brand, int year, boolean hasSidecar) {
        super(brand, year);
        this.hasSidecar = hasSidecar;
    }
    
    @Override
    public void start() {
        System.out.println("Motorcycle starting: Kick start or electric start");
    }
    
    @Override
    public void stop() {
        System.out.println("Motorcycle stopping: Hand brake and foot brake");
    }
    
    @Override
    public double calculateFuelEfficiency() {
        return 40.0;  // km per liter - more efficient
    }
    
    @Override
    public int getMaxSpeed() {
        return hasSidecar ? 120 : 200;  // km/h
    }
    
    public void wheelie() {
        System.out.println("Performing wheelie!");
    }
}

// Usage
// Vehicle v = new Vehicle("Generic", 2024);  // ERROR: Cannot instantiate abstract class

Vehicle car = new Car("Toyota", 2024, 4, "Automatic");
Vehicle bike = new Motorcycle("Honda", 2024, false);

car.displayInfo();
car.start();
car.refuel(50);
System.out.println("Fuel efficiency: " + car.calculateFuelEfficiency() + " km/L");

bike.displayInfo();
bike.start();
System.out.println("Max speed: " + bike.getMaxSpeed() + " km/h");
```

## Interfaces

**Interface** = A contract that defines:
- What methods a class must implement
- But NOT how to implement them
- All methods are public abstract by default (before Java 8)
- All fields are public static final by default

```java
interface Flyable {
    // All methods are public abstract by default
    void takeOff();
    void fly();
    void land();
    
    // Constants (public static final by default)
    int MAX_ALTITUDE = 40000;  // feet
    int MIN_ALTITUDE = 1000;
}

interface Swimmable {
    void swim();
    void dive();
    int getMaxDepth();
}

interface Walkable {
    void walk();
    void run();
}

// A class can implement multiple interfaces
class Duck implements Flyable, Swimmable, Walkable {
    private String name;
    
    public Duck(String name) {
        this.name = name;
    }
    
    // Implement Flyable
    @Override
    public void takeOff() {
        System.out.println(name + " flapping wings to take off from water");
    }
    
    @Override
    public void fly() {
        System.out.println(name + " flying at low altitude");
    }
    
    @Override
    public void land() {
        System.out.println(name + " landing gracefully on water");
    }
    
    // Implement Swimmable
    @Override
    public void swim() {
        System.out.println(name + " swimming on water surface");
    }
    
    @Override
    public void dive() {
        System.out.println(name + " diving underwater for food");
    }
    
    @Override
    public int getMaxDepth() {
        return 10;  // meters
    }
    
    // Implement Walkable
    @Override
    public void walk() {
        System.out.println(name + " waddling on land");
    }
    
    @Override
    public void run() {
        System.out.println(name + " running awkwardly");
    }
}

class Airplane implements Flyable {
    private String model;
    
    public Airplane(String model) {
        this.model = model;
    }
    
    @Override
    public void takeOff() {
        System.out.println(model + " accelerating on runway, lifting off");
    }
    
    @Override
    public void fly() {
        System.out.println(model + " cruising at " + MAX_ALTITUDE + " feet");
    }
    
    @Override
    public void land() {
        System.out.println(model + " descending and landing on runway");
    }
}

class Fish implements Swimmable {
    private String species;
    
    public Fish(String species) {
        this.species = species;
    }
    
    @Override
    public void swim() {
        System.out.println(species + " swimming through water");
    }
    
    @Override
    public void dive() {
        System.out.println(species + " diving to deeper water");
    }
    
    @Override
    public int getMaxDepth() {
        return 1000;  // meters
    }
}

// Usage - Polymorphism with interfaces
Flyable duck = new Duck("Donald");
Flyable plane = new Airplane("Boeing 747");

duck.fly();   // "Donald flying at low altitude"
plane.fly();  // "Boeing 747 cruising at 40000 feet"

// Duck can be treated as Swimmable too
Swimmable swimmer = new Duck("Daffy");
swimmer.swim();  // "Daffy swimming on water surface"

// Array of Flyable objects
Flyable[] flyingThings = {
    new Duck("Duck1"),
    new Airplane("Plane1"),
    new Duck("Duck2")
};

for (Flyable thing : flyingThings) {
    thing.takeOff();
    thing.fly();
    thing.land();
    System.out.println();
}
```

## Abstract Class vs Interface: Complete Comparison

| Feature | Abstract Class | Interface |
|---------|---------------|-----------|
| **Multiple Inheritance** | No (extends one class) | Yes (implements multiple) |
| **Constructor** | Yes | No |
| **Fields** | Any (private, protected, public) | Only public static final |
| **Method Types** | Abstract + Concrete | Abstract (default/static in Java 8+) |
| **Access Modifiers** | Any | Only public (or default in Java 9+) |
| **Inheritance Keyword** | extends | implements |
| **When to Use** | IS-A relationship with shared code | CAN-DO capability |
| **State** | Can have instance variables | Cannot have instance variables |

### Decision Tree: When to Use What?

```
Question 1: Do classes share common code/state?
├─ YES → Use Abstract Class
│   Example: Vehicle (all have brand, year, fuelLevel)
│
└─ NO → Use Interface
    Example: Flyable (planes, birds, drones - completely different implementations)

Question 2: Do you need multiple inheritance?
└─ YES → Use Interface
    Example: Duck implements Flyable, Swimmable, Walkable

Question 3: Is it a capability/behavior?
└─ YES → Use Interface
    Example: Comparable, Serializable, Runnable

Question 4: Is it a family of related classes?
└─ YES → Use Abstract Class
    Example: Shape → Circle, Rectangle, Triangle
```

### Real-World Example: Payment System

```java
// Interface - defines capability
interface PaymentMethod {
    boolean processPayment(double amount);
    String getPaymentDetails();
    boolean refund(double amount);
}

// Abstract class - shared behavior for online payments
abstract class OnlinePayment implements PaymentMethod {
    protected String transactionId;
    protected String customerEmail;
    
    public OnlinePayment(String customerEmail) {
        this.customerEmail = customerEmail;
    }
    
    // Common method for all online payments
    protected void generateTransactionId() {
        transactionId = "TXN" + System.currentTimeMillis();
    }
    
    protected void sendConfirmationEmail() {
        System.out.println("Sending confirmation to: " + customerEmail);
    }
    
    // Abstract method - each payment type implements differently
    public abstract boolean validatePayment();
    
    @Override
    public boolean refund(double amount) {
        System.out.println("Processing refund of $" + amount);
        return true;
    }
}

class CreditCard extends OnlinePayment {
    private String cardNumber;
    private String cvv;
    private String expiryDate;
    
    public CreditCard(String customerEmail, String cardNumber, String cvv, String expiryDate) {
        super(customerEmail);
        this.cardNumber = cardNumber;
        this.cvv = cvv;
        this.expiryDate = expiryDate;
    }
    
    @Override
    public boolean validatePayment() {
        // Validate card number, CVV, expiry
        return cardNumber.length() == 16 && cvv.length() == 3;
    }
    
    @Override
    public boolean processPayment(double amount) {
        generateTransactionId();
        if (validatePayment()) {
            System.out.println("Processing $" + amount + " via Credit Card");
            System.out.println("Card: **** **** **** " + cardNumber.substring(12));
            System.out.println("Transaction ID: " + transactionId);
            sendConfirmationEmail();
            return true;
        }
        return false;
    }
    
    @Override
    public String getPaymentDetails() {
        return "Credit Card ending in " + cardNumber.substring(12);
    }
}

class PayPal extends OnlinePayment {
    private String email;
    private String password;
    
    public PayPal(String customerEmail, String email, String password) {
        super(customerEmail);
        this.email = email;
        this.password = password;
    }
    
    @Override
    public boolean validatePayment() {
        return email.contains("@") && password.length() >= 8;
    }
    
    @Override
    public boolean processPayment(double amount) {
        generateTransactionId();
        if (validatePayment()) {
            System.out.println("Processing $" + amount + " via PayPal");
            System.out.println("PayPal account: " + email);
            System.out.println("Transaction ID: " + transactionId);
            sendConfirmationEmail();
            return true;
        }
        return false;
    }
    
    @Override
    public String getPaymentDetails() {
        return "PayPal account: " + email;
    }
}

// Cash doesn't extend OnlinePayment - different category
class Cash implements PaymentMethod {
    private double receivedAmount;
    
    @Override
    public boolean processPayment(double amount) {
        System.out.println("Received $" + amount + " in cash");
        receivedAmount = amount;
        return true;
    }
    
    @Override
    public String getPaymentDetails() {
        return "Cash payment: $" + receivedAmount;
    }
    
    @Override
    public boolean refund(double amount) {
        System.out.println("Refunding $" + amount + " in cash");
        return true;
    }
}

// Payment processor - works with any payment method
class PaymentProcessor {
    public void checkout(PaymentMethod payment, double amount) {
        System.out.println("\n=== CHECKOUT ===");
        System.out.println("Payment method: " + payment.getPaymentDetails());
        System.out.println("Amount: $" + amount);
        
        if (payment.processPayment(amount)) {
            System.out.println("✓ Payment successful!");
        } else {
            System.out.println("✗ Payment failed!");
        }
    }
}

// Usage
PaymentProcessor processor = new PaymentProcessor();

processor.checkout(
    new CreditCard("john@example.com", "1234567890123456", "123", "12/25"),
    99.99
);

processor.checkout(
    new PayPal("john@example.com", "john.paypal@example.com", "securepass123"),
    49.99
);

processor.checkout(
    new Cash(),
    29.99
);
```

---

# 5. Access Modifiers

Access modifiers control the visibility and accessibility of classes, methods, and variables.

## The Four Access Levels

```java
class AccessDemo {
    private int privateVar = 1;      // Only within this class
    int defaultVar = 2;              // Within same package (package-private)
    protected int protectedVar = 3;  // Within package + subclasses
    public int publicVar = 4;        // Everywhere
    
    private void privateMethod() {
        System.out.println("Private method");
    }
    
    void defaultMethod() {
        System.out.println("Default method");
    }
    
    protected void protectedMethod() {
        System.out.println("Protected method");
    }
    
    public void publicMethod() {
        System.out.println("Public method");
    }
    
    public void testAccess() {
        // Within same class - ALL accessible
        privateVar = 10;
        defaultVar = 20;
        protectedVar = 30;
        publicVar = 40;
        
        privateMethod();
        defaultMethod();
        protectedMethod();
        publicMethod();
    }
}

// In same package
class SamePackage {
    void test() {
        AccessDemo obj = new AccessDemo();
        
        // obj.privateVar = 10;     // ✗ ERROR: private
        obj.defaultVar = 20;        // ✓ OK: same package
        obj.protectedVar = 30;      // ✓ OK: same package
        obj.publicVar = 40;         // ✓ OK: public
        
        // obj.privateMethod();     // ✗ ERROR
        obj.defaultMethod();        // ✓ OK
        obj.protectedMethod();      // ✓ OK
        obj.publicMethod();         // ✓ OK
    }
}

// In different package, not subclass
package other;
import original.AccessDemo;

class DifferentPackage {
    void test() {
        AccessDemo obj = new AccessDemo();
        
        // obj.privateVar = 10;     // ✗ ERROR
        // obj.defaultVar = 20;     // ✗ ERROR: different package
        // obj.protectedVar = 30;   // ✗ ERROR: not subclass
        obj.publicVar = 40;         // ✓ OK: public
        
        // obj.privateMethod();     // ✗ ERROR
        // obj.defaultMethod();     // ✗ ERROR
        // obj.protectedMethod();   // ✗ ERROR
        obj.publicMethod();         // ✓ OK
    }
}

// In different package, subclass
package other;
import original.AccessDemo;

class SubclassInDifferentPackage extends AccessDemo {
    void test() {
        // Accessing through inheritance
        // privateVar = 10;         // ✗ ERROR: private not inherited
        // defaultVar = 20;         // ✗ ERROR: different package
        protectedVar = 30;          // ✓ OK: inherited protected
        publicVar = 40;             // ✓ OK: public
        
        // privateMethod();         // ✗ ERROR
        // defaultMethod();         // ✗ ERROR
        protectedMethod();          // ✓ OK: inherited
        publicMethod();             // ✓ OK
        
        // Accessing through object reference
        AccessDemo obj = new AccessDemo();
        // obj.protectedVar = 30;   // ✗ ERROR: not through inheritance
        obj.publicVar = 40;         // ✓ OK
    }
}
```

## Access Modifier Summary Table

| Modifier | Same Class | Same Package | Subclass (Different Package) | Everywhere |
|----------|-----------|--------------|------------------------------|------------|
| **private** | ✓ | ✗ | ✗ | ✗ |
| **default** (no modifier) | ✓ | ✓ | ✗ | ✗ |
| **protected** | ✓ | ✓ | ✓ (through inheritance) | ✗ |
| **public** | ✓ | ✓ | ✓ | ✓ |

## Best Practices

1. **Start with private** - Make everything private by default
2. **Use protected for inheritance** - When subclasses need access
3. **Use public sparingly** - Only for the public API
4. **Avoid default** - Be explicit with access modifiers

```java
class BestPracticeExample {
    // Private fields - encapsulation
    private String name;
    private int age;
    
    // Public constructor
    public BestPracticeExample(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    // Public getters/setters - controlled access
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        if (name != null && !name.trim().isEmpty()) {
            this.name = name;
        }
    }
    
    // Private helper method
    private boolean isValidAge(int age) {
        return age >= 0 && age <= 150;
    }
    
    // Public method using private helper
    public void setAge(int age) {
        if (isValidAge(age)) {
            this.age = age;
        }
    }
    
    // Protected method for subclasses
    protected void displayInfo() {
        System.out.println(name + ", " + age);
    }
}
```

---

## Summary: Core OOP Principles

1. **Encapsulation**: Hide data, provide controlled access through methods
2. **Inheritance**: Reuse code through IS-A relationships
3. **Polymorphism**: One interface, many implementations
4. **Abstraction**: Hide complexity, show only essentials

These four principles work together to create maintainable, flexible, and scalable code!