# Part 2: SOLID Principles - Complete Guide

## Table of Contents
1. [Introduction to SOLID](#introduction-to-solid)
2. [Single Responsibility Principle (SRP)](#single-responsibility-principle-srp)
3. [Open/Closed Principle (OCP)](#openclosed-principle-ocp)
4. [Liskov Substitution Principle (LSP)](#liskov-substitution-principle-lsp)
5. [Interface Segregation Principle (ISP)](#interface-segregation-principle-isp)
6. [Dependency Inversion Principle (DIP)](#dependency-inversion-principle-dip)
7. [Summary & Real-World Application](#summary--real-world-application)

---

# Introduction to SOLID

**SOLID** is an acronym for five design principles that make software:
- **More maintainable** - easier to modify
- **More flexible** - easier to extend
- **More scalable** - easier to grow
- **More testable** - easier to test
- **Less fragile** - less likely to break

These principles were introduced by **Robert C. Martin (Uncle Bob)** and are fundamental to object-oriented design.

**Think of SOLID as the foundation of good software architecture** - like building a house on solid ground rather than sand.

---

# Single Responsibility Principle (SRP)

## Definition
**A class should have only ONE reason to change** - it should have only one job or responsibility.

## Real-World Analogy
Think of a restaurant:
- **Chef** - cooks food (one responsibility)
- **Waiter** - serves customers (one responsibility)
- **Cashier** - handles payments (one responsibility)

If the chef also had to serve tables and handle money, it would be chaos! Each person has ONE clear responsibility.

## Why SRP Matters
- ✅ Easier to understand
- ✅ Easier to test
- ✅ Easier to maintain
- ✅ Changes in one area don't affect others
- ✅ Reduces coupling

## ❌ BAD Example - God Object Anti-Pattern

```java
// This class does EVERYTHING - violates SRP!
class Employee {
    private String name;
    private String email;
    private double salary;
    
    // Responsibility 1: Data management
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    
    // Responsibility 2: Business logic
    public double calculateTax() {
        return salary * 0.2;
    }
    
    public double calculateBonus() {
        return salary * 0.1;
    }
    
    // Responsibility 3: Database
    public void saveToDatabase() {
        System.out.println("INSERT INTO employees...");
    }
    
    // Responsibility 4: Email
    public void sendPayslipEmail() {
        System.out.println("Sending email...");
    }
    
    // Responsibility 5: Reporting
    public void generatePayslip() {
        System.out.println("=== PAYSLIP ===");
    }
}
```

**Problems:**
- 5 reasons to change this class!
- Tax rules change → modify Employee
- Database changes → modify Employee
- Email template changes → modify Employee
- Hard to test each part independently

## ✅ GOOD Example - Following SRP

```java
// 1. Data only
class Employee {
    private String id;
    private String name;
    private double salary;
    
    public Employee(String id, String name, double salary) {
        this.id = id;
        this.name = name;
        this.salary = salary;
    }
    
    public String getId() { return id; }
    public String getName() { return name; }
    public double getSalary() { return salary; }
}

// 2. Business logic
class SalaryCalculator {
    public double calculateTax(Employee emp) {
        return emp.getSalary() * 0.2;
    }
    
    public double calculateBonus(Employee emp) {
        return emp.getSalary() * 0.1;
    }
}

// 3. Database operations
class EmployeeRepository {
    public void save(Employee emp) {
        System.out.println("Saving: " + emp.getName());
    }
}

// 4. Email service
class EmailService {
    public void sendPayslip(Employee emp) {
        System.out.println("Emailing payslip to: " + emp.getName());
    }
}

// 5. Report generation
class PayslipGenerator {
    public String generate(Employee emp, SalaryCalculator calc) {
        return "Payslip for " + emp.getName() + 
               "\nSalary: $" + emp.getSalary() +
               "\nTax: $" + calc.calculateTax(emp);
    }
}
```

**Benefits:**
- Each class has ONE clear responsibility
- Easy to test each class separately
- Changes isolated to specific classes
- Can reuse components

---

# Open/Closed Principle (OCP)

## Definition
**Software entities should be OPEN for extension but CLOSED for modification.**

Translation: Add new features WITHOUT changing existing code.

## Real-World Analogy
Think of a smartphone:
- You can add new apps (extension) ✅
- Without modifying the phone's OS (modification) ✅

## Why OCP Matters
- ✅ Reduces risk of breaking existing code
- ✅ Makes code more stable
- ✅ Easier to add features
- ✅ Promotes code reuse

## ❌ BAD Example - Modification Required

```java
class AreaCalculator {
    public double calculateArea(Object shape) {
        if (shape instanceof Circle) {
            Circle c = (Circle) shape;
            return Math.PI * c.radius * c.radius;
        } 
        else if (shape instanceof Rectangle) {
            Rectangle r = (Rectangle) shape;
            return r.width * r.height;
        }
        // Adding Triangle? Must MODIFY this method!
        else if (shape instanceof Triangle) {
            Triangle t = (Triangle) shape;
            return 0.5 * t.base * t.height;
        }
        return 0;
    }
}
```

**Problem:** Every new shape requires modifying AreaCalculator!

## ✅ GOOD Example - Extension Without Modification

```java
// Define contract
interface Shape {
    double calculateArea();
}

// Each shape implements its own logic
class Circle implements Shape {
    private double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}

class Rectangle implements Shape {
    private double width, height;
    
    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }
    
    @Override
    public double calculateArea() {
        return width * height;
    }
}

// Add new shape - NO modification needed!
class Triangle implements Shape {
    private double base, height;
    
    public Triangle(double base, double height) {
        this.base = base;
        this.height = height;
    }
    
    @Override
    public double calculateArea() {
        return 0.5 * base * height;
    }
}

// Calculator never changes!
class AreaCalculator {
    public double calculateArea(Shape shape) {
        return shape.calculateArea();
    }
    
    public double totalArea(List<Shape> shapes) {
        return shapes.stream()
                     .mapToDouble(Shape::calculateArea)
                     .sum();
    }
}
```

## Real-World Example: Payment Methods

```java
// Payment interface
interface PaymentMethod {
    void process(double amount);
}

// Existing payments
class CreditCard implements PaymentMethod {
    @Override
    public void process(double amount) {
        System.out.println("Processing $" + amount + " via Credit Card");
    }
}

class PayPal implements PaymentMethod {
    @Override
    public void process(double amount) {
        System.out.println("Processing $" + amount + " via PayPal");
    }
}

// Add new payment - NO modification to existing code!
class Bitcoin implements PaymentMethod {
    @Override
    public void process(double amount) {
        System.out.println("Processing $" + amount + " via Bitcoin");
    }
}

class ApplePay implements PaymentMethod {
    @Override
    public void process(double amount) {
        System.out.println("Processing $" + amount + " via Apple Pay");
    }
}

// Processor works with any payment method
class PaymentProcessor {
    public void checkout(PaymentMethod method, double amount) {
        method.process(amount);
    }
}
```

---

# Liskov Substitution Principle (LSP)

## Definition
**Objects of a superclass should be replaceable with objects of a subclass without breaking the application.**

Translation: If B is a subclass of A, you should be able to use B anywhere you use A.

## Real-World Analogy
If you have a "Vehicle" parking spot:
- You can park a Car ✅
- You can park a Motorcycle ✅
- You can park a Truck ✅

All vehicles should work in a vehicle spot!

## The Rectangle-Square Problem (Classic Violation)

### ❌ BAD Example

```java
class Rectangle {
    protected int width;
    protected int height;
    
    public void setWidth(int width) {
        this.width = width;
    }
    
    public void setHeight(int height) {
        this.height = height;
    }
    
    public int getArea() {
        return width * height;
    }
}

class Square extends Rectangle {
    @Override
    public void setWidth(int width) {
        this.width = width;
        this.height = width;  // Keep equal!
    }
    
    @Override
    public void setHeight(int height) {
        this.width = height;  // Keep equal!
        this.height = height;
    }
}

// Test code
void testRectangle(Rectangle rect) {
    rect.setWidth(5);
    rect.setHeight(4);
    
    // Expected: 5 * 4 = 20
    assert rect.getArea() == 20;
}

// Usage
Rectangle rect = new Rectangle();
testRectangle(rect);  // ✓ Works - area is 20

Rectangle square = new Square();
testRectangle(square);  // ✗ FAILS - area is 16!
```

**Problem:** Square changes Rectangle's behavior!

### ✅ GOOD Example

```java
// Common interface
interface Shape {
    int getArea();
}

class Rectangle implements Shape {
    private int width, height;
    
    public Rectangle(int width, int height) {
        this.width = width;
        this.height = height;
    }
    
    public void setWidth(int width) { this.width = width; }
    public void setHeight(int height) { this.height = height; }
    
    @Override
    public int getArea() {
        return width * height;
    }
}

class Square implements Shape {
    private int side;
    
    public Square(int side) {
        this.side = side;
    }
    
    public void setSide(int side) { this.side = side; }
    
    @Override
    public int getArea() {
        return side * side;
    }
}

// Both work correctly as Shape
void printArea(Shape shape) {
    System.out.println("Area: " + shape.getArea());
}
```

## Real-World Example: Birds

### ❌ BAD - Penguins Can't Fly!

```java
class Bird {
    public void fly() {
        System.out.println("Flying...");
    }
}

class Sparrow extends Bird {
    // Can fly - OK
}

class Penguin extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Can't fly!");
    }
}

// Code expects all birds to fly
void makeBirdFly(Bird bird) {
    bird.fly();  // Breaks with Penguin!
}
```

### ✅ GOOD - Separate Capabilities

```java
class Bird {
    public void eat() {
        System.out.println("Eating...");
    }
}

interface Flyable {
    void fly();
}

class Sparrow extends Bird implements Flyable {
    @Override
    public void fly() {
        System.out.println("Sparrow flying");
    }
}

class Penguin extends Bird {
    public void swim() {
        System.out.println("Penguin swimming");
    }
}

// Explicit about requirements
void makeFly(Flyable bird) {
    bird.fly();
}

// Usage
makeFly(new Sparrow());  // ✓ Works
// makeFly(new Penguin());  // ✓ Compile error - clear!
```

---

# Interface Segregation Principle (ISP)

## Definition
**Clients should not be forced to depend on interfaces they don't use.**

Translation: Don't create fat interfaces. Make small, focused ones.

## Real-World Analogy
A TV remote:
- Basic remote: Power, Volume, Channel (simple) ✅
- Universal remote: 100 buttons (overwhelming) ❌

Give users only what they need!

## ❌ BAD Example - Fat Interface

```java
// Fat interface - too many methods!
interface Worker {
    void work();
    void eat();
    void sleep();
    void getSalary();
    void attendMeeting();
}

class HumanWorker implements Worker {
    @Override
    public void work() { System.out.println("Working"); }
    @Override
    public void eat() { System.out.println("Eating"); }
    @Override
    public void sleep() { System.out.println("Sleeping"); }
    @Override
    public void getSalary() { System.out.println("Getting paid"); }
    @Override
    public void attendMeeting() { System.out.println("In meeting"); }
}

class RobotWorker implements Worker {
    @Override
    public void work() { System.out.println("Working 24/7"); }
    
    @Override
    public void eat() {
        throw new UnsupportedOperationException("Robots don't eat!");
    }
    
    @Override
    public void sleep() {
        throw new UnsupportedOperationException("Robots don't sleep!");
    }
    
    @Override
    public void getSalary() {
        throw new UnsupportedOperationException("Robots don't get paid!");
    }
    
    @Override
    public void attendMeeting() {
        throw new UnsupportedOperationException("Robots don't attend meetings!");
    }
}
```

**Problem:** RobotWorker forced to implement methods it doesn't need!

## ✅ GOOD Example - Segregated Interfaces

```java
// Small, focused interfaces
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

interface Sleepable {
    void sleep();
}

interface Payable {
    void getSalary();
}

// Human implements what it needs
class HumanWorker implements Workable, Eatable, Sleepable, Payable {
    @Override
    public void work() { System.out.println("Working"); }
    @Override
    public void eat() { System.out.println("Eating"); }
    @Override
    public void sleep() { System.out.println("Sleeping"); }
    @Override
    public void getSalary() { System.out.println("Getting paid"); }
}

// Robot only implements what it needs
class RobotWorker implements Workable {
    @Override
    public void work() { System.out.println("Working 24/7"); }
}

// Manager for workable entities
class WorkManager {
    public void manage(Workable worker) {
        worker.work();  // Only depends on Workable
    }
}
```

## Real-World Example: Printers

```java
// Segregated interfaces
interface Printer {
    void print(Document doc);
}

interface Scanner {
    void scan(Document doc);
}

interface Fax {
    void fax(Document doc);
}

// Simple printer - only what it can do
class SimplePrinter implements Printer {
    @Override
    public void print(Document doc) {
        System.out.println("Printing: " + doc.getName());
    }
}

// Multi-function device - implements multiple
class MultiFunctionPrinter implements Printer, Scanner, Fax {
    @Override
    public void print(Document doc) {
        System.out.println("Printing: " + doc.getName());
    }
    
    @Override
    public void scan(Document doc) {
        System.out.println("Scanning: " + doc.getName());
    }
    
    @Override
    public void fax(Document doc) {
        System.out.println("Faxing: " + doc.getName());
    }
}

class Document {
    private String name;
    public Document(String name) { this.name = name; }
    public String getName() { return name; }
}
```

---

# Dependency Inversion Principle (DIP)

## Definition
1. **High-level modules should not depend on low-level modules. Both should depend on abstractions.**
2. **Abstractions should not depend on details. Details should depend on abstractions.**

Translation: Depend on interfaces, not concrete classes.

## Real-World Analogy
Think of electrical outlets:
- You plug any device into a standard outlet (abstraction) ✅
- You don't need a custom outlet for each device ✅

The outlet (interface) is the abstraction!

## ❌ BAD Example - Tight Coupling

```java
// Low-level module
class MySQLDatabase {
    public void save(String data) {
        System.out.println("Saving to MySQL: " + data);
    }
}

// High-level module depends on low-level
class UserService {
    private MySQLDatabase database;  // Tight coupling!
    
    public UserService() {
        this.database = new MySQLDatabase();
    }
    
    public void saveUser(String userData) {
        database.save(userData);
    }
}

// Problem: Can't switch to PostgreSQL without modifying UserService!
```

**Problems:**
- UserService tightly coupled to MySQLDatabase
- Hard to test (can't mock database)
- Can't switch databases easily
- Violates Open/Closed Principle

## ✅ GOOD Example - Dependency Injection

```java
// Abstraction (interface)
interface Database {
    void save(String data);
    String retrieve(String id);
}

// Low-level modules implement abstraction
class MySQLDatabase implements Database {
    @Override
    public void save(String data) {
        System.out.println("Saving to MySQL: " + data);
    }
    
    @Override
    public String retrieve(String id) {
        return "Data from MySQL";
    }
}

class PostgreSQLDatabase implements Database {
    @Override
    public void save(String data) {
        System.out.println("Saving to PostgreSQL: " + data);
    }
    
    @Override
    public String retrieve(String id) {
        return "Data from PostgreSQL";
    }
}

class MongoDatabase implements Database {
    @Override
    public void save(String data) {
        System.out.println("Saving to MongoDB: " + data);
    }
    
    @Override
    public String retrieve(String id) {
        return "Data from MongoDB";
    }
}

// High-level module depends on abstraction
class UserService {
    private Database database;  // Depends on interface!
    
    // Dependency Injection via constructor
    public UserService(Database database) {
        this.database = database;
    }
    
    public void saveUser(String userData) {
        database.save(userData);
    }
    
    public String getUser(String id) {
        return database.retrieve(id);
    }
}

// Usage - Easy to switch implementations!
Database mysql = new MySQLDatabase();
UserService service1 = new UserService(mysql);
service1.saveUser("John");

Database postgres = new PostgreSQLDatabase();
UserService service2 = new UserService(postgres);
service2.saveUser("Jane");

Database mongo = new MongoDatabase();
UserService service3 = new UserService(mongo);
service3.saveUser("Bob");
```

## Types of Dependency Injection

```java
interface Logger {
    void log(String message);
}

class ConsoleLogger implements Logger {
    @Override
    public void log(String message) {
        System.out.println("LOG: " + message);
    }
}

class FileLogger implements Logger {
    @Override
    public void log(String message) {
        System.out.println("Writing to file: " + message);
    }
}

class OrderService {
    private Logger logger;
    
    // 1. Constructor Injection (RECOMMENDED)
    public OrderService(Logger logger) {
        this.logger = logger;
    }
    
    // 2. Setter Injection
    public void setLogger(Logger logger) {
        this.logger = logger;
    }
    
    public void placeOrder(String orderId) {
        logger.log("Order placed: " + orderId);
    }
}

// Usage
// Constructor injection
OrderService service1 = new OrderService(new ConsoleLogger());
service1.placeOrder("ORD-001");

// Setter injection
OrderService service2 = new OrderService(new ConsoleLogger());
service2.setLogger(new FileLogger());
service2.placeOrder("ORD-002");
```

## Real-World Example: Notification System

```java
// Abstraction
interface NotificationService {
    void send(String recipient, String message);
}

// Implementations
class EmailNotification implements NotificationService {
    @Override
    public void send(String recipient, String message) {
        System.out.println("Email to " + recipient + ": " + message);
    }
}

class SMSNotification implements NotificationService {
    @Override
    public void send(String recipient, String message) {
        System.out.println("SMS to " + recipient + ": " + message);
    }
}

class PushNotification implements NotificationService {
    @Override
    public void send(String recipient, String message) {
        System.out.println("Push to " + recipient + ": " + message);
    }
}

// High-level module
class UserRegistration {
    private NotificationService notifier;
    
    public UserRegistration(NotificationService notifier) {
        this.notifier = notifier;
    }
    
    public void registerUser(String username, String contact) {
        // Registration logic
        System.out.println("Registering user: " + username);
        
        // Send notification
        notifier.send(contact, "Welcome " + username + "!");
    }
}

// Usage - Easy to switch notification methods
UserRegistration emailReg = new UserRegistration(new EmailNotification());
emailReg.registerUser("John", "john@example.com");

UserRegistration smsReg = new UserRegistration(new SMSNotification());
smsReg.registerUser("Jane", "+1234567890");

UserRegistration pushReg = new UserRegistration(new PushNotification());
pushReg.registerUser("Bob", "bob_device_id");
```

---

# Summary & Real-World Application

## SOLID Principles Quick Reference

| Principle | What It Means | Key Benefit |
|-----------|---------------|-------------|
| **S**RP | One class, one responsibility | Easy to maintain |
| **O**CP | Open for extension, closed for modification | Add features safely |
| **L**SP | Subclasses should work like parent | Reliable inheritance |
| **I**SP | Small, focused interfaces | No unused methods |
| **D**IP | Depend on abstractions | Flexible, testable |

## How They Work Together

```java
// Complete example showing all SOLID principles

// DIP - Depend on abstractions
interface PaymentGateway {
    boolean process(double amount);
}

interface NotificationService {
    void notify(String message);
}

// OCP - Open for extension
class StripePayment implements PaymentGateway {
    @Override
    public boolean process(double amount) {
        System.out.println("Processing $" + amount + " via Stripe");
        return true;
    }
}

class PayPalPayment implements PaymentGateway {
    @Override
    public boolean process(double amount) {
        System.out.println("Processing $" + amount + " via PayPal");
        return true;
    }
}

// ISP - Focused interfaces
class EmailNotification implements NotificationService {
    @Override
    public void notify(String message) {
        System.out.println("Email: " + message);
    }
}

// SRP - Single responsibility
class Order {
    private String id;
    private double amount;
    
    public Order(String id, double amount) {
        this.id = id;
        this.amount = amount;
    }
    
    public String getId() { return id; }
    public double getAmount() { return amount; }
}

// SRP - Order processing only
class OrderProcessor {
    private PaymentGateway paymentGateway;
    private NotificationService notifier;
    
    // DIP - Inject dependencies
    public OrderProcessor(PaymentGateway gateway, NotificationService notifier) {
        this.paymentGateway = gateway;
        this.notifier = notifier;
    }
    
    public void processOrder(Order order) {
        if (paymentGateway.process(order.getAmount())) {
            notifier.notify("Order " + order.getId() + " processed successfully");
        }
    }
}

// LSP - Any PaymentGateway works
public class Main {
    public static void main(String[] args) {
        Order order = new Order("ORD-001", 99.99);
        
        // Can switch implementations easily
        OrderProcessor processor1 = new OrderProcessor(
            new StripePayment(),
            new EmailNotification()
        );
        processor1.processOrder(order);
        
        OrderProcessor processor2 = new OrderProcessor(
            new PayPalPayment(),
            new EmailNotification()
        );
        processor2.processOrder(order);
    }
}
```

## Common Violations to Avoid

### ❌ God Object (Violates SRP)
```java
class Application {
    // Does everything - BAD!
    void connectDatabase() { }
    void sendEmail() { }
    void processPayment() { }
    void generateReport() { }
}
```

### ❌ Rigid Code (Violates OCP)
```java
if (type == "A") { /* ... */ }
else if (type == "B") { /* ... */ }
// Adding type C requires modification!
```

### ❌ Broken Substitution (Violates LSP)
```java
class Parent {
    void method() { /* works */ }
}

class Child extends Parent {
    @Override
    void method() {
        throw new Exception();  // Breaks parent's contract!
    }
}
```

### ❌ Fat Interface (Violates ISP)
```java
interface Everything {
    void method1();
    void method2();
    void method3();
    // ... 20 more methods
}
```

### ❌ Tight Coupling (Violates DIP)
```java
class Service {
    private ConcreteClass dependency = new ConcreteClass();
    // Can't change or test easily!
}
```

## Benefits of Following SOLID

1. **Maintainability** - Code is easier to understand and modify
2. **Testability** - Each component can be tested independently
3. **Flexibility** - Easy to add new features
4. **Reusability** - Components can be reused in different contexts
5. **Scalability** - System can grow without becoming fragile

## When to Apply SOLID

✅ **Apply when:**
- Building new systems
- Refactoring existing code
- Code is becoming hard to maintain
- Adding new features frequently

❌ **Don't over-engineer:**
- Simple scripts or prototypes
- Code that rarely changes
- When deadlines are tight (but plan to refactor later)

## Practice Exercise

Try refactoring this code to follow SOLID:

```java
class BlogPost {
    private String title;
    private String content;
    
    public void save() {
        // Database code
    }
    
    public void display() {
        // HTML rendering
    }
    
    public void sendEmail() {
        // Email notification
    }
}
```

**Solution:** Split into:
- `BlogPost` (data)
- `BlogPostRepository` (persistence)
- `BlogPostRenderer` (display)
- `EmailService` (notifications)

---

## Conclusion

SOLID principles are not rules, but **guidelines** that help you write better code. They work together to create software that is:
- Easy to understand
- Easy to change
- Easy to test
- Easy to extend

**Remember:** Good design is about making future changes easy!