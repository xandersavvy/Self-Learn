# Part 5: Multithreading & Architecture - Complete Guide

## Table of Contents
1. [Multithreading Basics](#multithreading-basics)
2. [Thread Safety](#thread-safety)
3. [Synchronization](#synchronization)
4. [Volatile Keyword](#volatile-keyword)
5. [Immutable Objects](#immutable-objects)
6. [Architecture & Design Thinking](#architecture--design-thinking)
7. [Dependency Injection](#dependency-injection)
8. [Design Principles](#design-principles)

---

# Multithreading Basics

## What is a Thread?

A **thread** is the smallest unit of execution within a process. Multiple threads can run concurrently, sharing the same memory space.

### Real-World Analogy
Think of a restaurant:
- **Process** = The restaurant
- **Threads** = Waiters serving different tables simultaneously
- **Shared Memory** = Kitchen (shared resource)

## Creating Threads in Java

### Method 1: Extending Thread Class

```java
class MyThread extends Thread {
    private String name;
    
    public MyThread(String name) {
        this.name = name;
    }
    
    @Override
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println(name + ": " + i);
            try {
                Thread.sleep(500);  // Sleep 500ms
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

// Usage
MyThread t1 = new MyThread("Thread-1");
MyThread t2 = new MyThread("Thread-2");

t1.start();  // Starts thread (calls run() in new thread)
t2.start();

// Output (interleaved):
// Thread-1: 1
// Thread-2: 1
// Thread-1: 2
// Thread-2: 2
// ...
```

### Method 2: Implementing Runnable (Preferred)

```java
class MyTask implements Runnable {
    private String name;
    
    public MyTask(String name) {
        this.name = name;
    }
    
    @Override
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println(name + ": " + i);
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

// Usage
Thread t1 = new Thread(new MyTask("Task-1"));
Thread t2 = new Thread(new MyTask("Task-2"));

t1.start();
t2.start();
```

### Method 3: Lambda Expression (Java 8+)

```java
Thread t1 = new Thread(() -> {
    for (int i = 1; i <= 5; i++) {
        System.out.println("Lambda-1: " + i);
        try {
            Thread.sleep(500);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
});

t1.start();
```

## Thread Lifecycle

```
NEW → RUNNABLE → RUNNING → TERMINATED
         ↓           ↓
      BLOCKED    WAITING
```

```java
class ThreadLifecycle {
    public static void main(String[] args) throws InterruptedException {
        Thread t = new Thread(() -> {
            System.out.println("Thread running");
            try {
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
        
        System.out.println("State: " + t.getState());  // NEW
        
        t.start();
        System.out.println("State: " + t.getState());  // RUNNABLE
        
        Thread.sleep(100);
        System.out.println("State: " + t.getState());  // TIMED_WAITING
        
        t.join();  // Wait for thread to finish
        System.out.println("State: " + t.getState());  // TERMINATED
    }
}
```

---

# Thread Safety

## The Problem: Race Condition

```java
// ❌ NOT Thread-Safe!
class Counter {
    private int count = 0;
    
    public void increment() {
        count++;  // NOT atomic! (read, increment, write)
    }
    
    public int getCount() {
        return count;
    }
}

// Problem demonstration
Counter counter = new Counter();

// Create 1000 threads, each incrementing 1000 times
Thread[] threads = new Thread[1000];
for (int i = 0; i < 1000; i++) {
    threads[i] = new Thread(() -> {
        for (int j = 0; j < 1000; j++) {
            counter.increment();
        }
    });
    threads[i].start();
}

// Wait for all threads
for (Thread t : threads) {
    t.join();
}

System.out.println("Count: " + counter.getCount());
// Expected: 1,000,000
// Actual: Less than 1,000,000 (race condition!)
```

### Why It Fails

```
Thread 1: Read count (0) → Increment (1) → Write (1)
Thread 2: Read count (0) → Increment (1) → Write (1)

Result: count = 1 (should be 2!)
```

---

# Synchronization

## synchronized Keyword

### Method-Level Synchronization

```java
// ✅ Thread-Safe with synchronized
class Counter {
    private int count = 0;
    
    // Synchronized method - only one thread at a time
    public synchronized void increment() {
        count++;
    }
    
    public synchronized int getCount() {
        return count;
    }
}

// Now it works correctly!
Counter counter = new Counter();

Thread[] threads = new Thread[1000];
for (int i = 0; i < 1000; i++) {
    threads[i] = new Thread(() -> {
        for (int j = 0; j < 1000; j++) {
            counter.increment();
        }
    });
    threads[i].start();
}

for (Thread t : threads) {
    t.join();
}

System.out.println("Count: " + counter.getCount());
// Output: 1,000,000 ✓
```

### Block-Level Synchronization

```java
class BankAccount {
    private double balance = 0;
    private final Object lock = new Object();
    
    public void deposit(double amount) {
        synchronized (lock) {  // Synchronized block
            balance += amount;
            System.out.println("Deposited: " + amount + ", Balance: " + balance);
        }
    }
    
    public void withdraw(double amount) {
        synchronized (lock) {
            if (balance >= amount) {
                balance -= amount;
                System.out.println("Withdrawn: " + amount + ", Balance: " + balance);
            } else {
                System.out.println("Insufficient funds");
            }
        }
    }
}
```

### Static Synchronization

```java
class DatabaseConnection {
    private static int connectionCount = 0;
    
    // Synchronized on class object
    public static synchronized void incrementConnections() {
        connectionCount++;
    }
    
    // Equivalent to:
    public static void incrementConnections2() {
        synchronized (DatabaseConnection.class) {
            connectionCount++;
        }
    }
}
```

## Deadlock Example

```java
// ❌ Deadlock situation!
class DeadlockDemo {
    private final Object lock1 = new Object();
    private final Object lock2 = new Object();
    
    public void method1() {
        synchronized (lock1) {
            System.out.println("Thread 1: Holding lock1...");
            try { Thread.sleep(100); } catch (InterruptedException e) {}
            
            System.out.println("Thread 1: Waiting for lock2...");
            synchronized (lock2) {
                System.out.println("Thread 1: Holding lock1 & lock2");
            }
        }
    }
    
    public void method2() {
        synchronized (lock2) {
            System.out.println("Thread 2: Holding lock2...");
            try { Thread.sleep(100); } catch (InterruptedException e) {}
            
            System.out.println("Thread 2: Waiting for lock1...");
            synchronized (lock1) {
                System.out.println("Thread 2: Holding lock1 & lock2");
            }
        }
    }
}

// Deadlock occurs:
// Thread 1 holds lock1, waits for lock2
// Thread 2 holds lock2, waits for lock1
// Both wait forever!
```

### Avoiding Deadlock

```java
// ✅ Solution: Always acquire locks in same order
class NoDeadlock {
    private final Object lock1 = new Object();
    private final Object lock2 = new Object();
    
    public void method1() {
        synchronized (lock1) {  // Always lock1 first
            synchronized (lock2) {  // Then lock2
                System.out.println("Method 1 executing");
            }
        }
    }
    
    public void method2() {
        synchronized (lock1) {  // Always lock1 first
            synchronized (lock2) {  // Then lock2
                System.out.println("Method 2 executing");
            }
        }
    }
}
```

---

# Volatile Keyword

## What is volatile?

**volatile** ensures that changes to a variable are immediately visible to all threads.

### Without volatile

```java
// ❌ Problem: Thread may cache the value
class TaskRunner {
    private boolean running = true;
    
    public void start() {
        new Thread(() -> {
            while (running) {  // May read cached value!
                // Do work
            }
            System.out.println("Thread stopped");
        }).start();
    }
    
    public void stop() {
        running = false;  // Other thread may not see this!
    }
}
```

### With volatile

```java
// ✅ Solution: Use volatile
class TaskRunner {
    private volatile boolean running = true;  // Visible to all threads
    
    public void start() {
        new Thread(() -> {
            while (running) {  // Always reads latest value
                // Do work
            }
            System.out.println("Thread stopped");
        }).start();
    }
    
    public void stop() {
        running = false;  // Immediately visible to all threads
    }
}
```

## volatile vs synchronized

| Feature | volatile | synchronized |
|---------|----------|--------------|
| **Visibility** | ✓ Guarantees | ✓ Guarantees |
| **Atomicity** | ✗ No | ✓ Yes |
| **Use Case** | Simple flags | Complex operations |
| **Performance** | Faster | Slower |

```java
class Example {
    private volatile int counter = 0;
    
    // ❌ NOT thread-safe (counter++ is not atomic)
    public void increment() {
        counter++;  // Read-modify-write (3 operations)
    }
    
    // ✅ Thread-safe
    public synchronized void incrementSafe() {
        counter++;
    }
}
```

---

# Immutable Objects

## Why Immutable Objects are Thread-Safe

**Immutable objects cannot be modified after creation**, so they're inherently thread-safe!

### ✅ Immutable Class Example

```java
public final class ImmutablePerson {
    private final String name;
    private final int age;
    private final List<String> hobbies;
    
    public ImmutablePerson(String name, int age, List<String> hobbies) {
        this.name = name;
        this.age = age;
        // Defensive copy to prevent external modification
        this.hobbies = new ArrayList<>(hobbies);
    }
    
    public String getName() {
        return name;
    }
    
    public int getAge() {
        return age;
    }
    
    public List<String> getHobbies() {
        // Return unmodifiable view
        return Collections.unmodifiableList(hobbies);
    }
    
    // To "modify", create new object
    public ImmutablePerson withAge(int newAge) {
        return new ImmutablePerson(this.name, newAge, this.hobbies);
    }
}

// Thread-safe usage
ImmutablePerson person = new ImmutablePerson("John", 30, Arrays.asList("Reading", "Gaming"));

// Multiple threads can safely read
Thread t1 = new Thread(() -> System.out.println(person.getName()));
Thread t2 = new Thread(() -> System.out.println(person.getAge()));

t1.start();
t2.start();
// No synchronization needed!
```

### Rules for Immutability

1. ✅ Class is `final` (cannot be extended)
2. ✅ All fields are `final`
3. ✅ All fields are `private`
4. ✅ No setters
5. ✅ Defensive copying for mutable fields
6. ✅ Return unmodifiable collections

---

# Architecture & Design Thinking

## Separation of Concerns

**Divide your application into distinct sections**, each addressing a separate concern.

### Example: Layered Architecture

```java
// ❌ BAD - Everything mixed together
class UserController {
    public void registerUser(String name, String email) {
        // Validation
        if (name == null || name.isEmpty()) {
            throw new IllegalArgumentException("Name required");
        }
        
        // Business logic
        String hashedPassword = hashPassword("default123");
        
        // Database
        Connection conn = DriverManager.getConnection("jdbc:...");
        PreparedStatement stmt = conn.prepareStatement("INSERT INTO users...");
        stmt.setString(1, name);
        stmt.setString(2, email);
        stmt.executeUpdate();
        
        // Email
        sendEmail(email, "Welcome!");
    }
}

// ✅ GOOD - Separated concerns
// 1. Controller Layer (HTTP handling)
class UserController {
    private UserService userService;
    
    public UserController(UserService userService) {
        this.userService = userService;
    }
    
    public void registerUser(String name, String email) {
        userService.register(name, email);
    }
}

// 2. Service Layer (Business logic)
class UserService {
    private UserRepository repository;
    private EmailService emailService;
    private PasswordHasher hasher;
    
    public UserService(UserRepository repository, EmailService emailService, PasswordHasher hasher) {
        this.repository = repository;
        this.emailService = emailService;
        this.hasher = hasher;
    }
    
    public void register(String name, String email) {
        // Validation
        validateUser(name, email);
        
        // Business logic
        String hashedPassword = hasher.hash("default123");
        User user = new User(name, email, hashedPassword);
        
        // Persistence
        repository.save(user);
        
        // Notification
        emailService.sendWelcomeEmail(email);
    }
    
    private void validateUser(String name, String email) {
        if (name == null || name.isEmpty()) {
            throw new IllegalArgumentException("Name required");
        }
        if (email == null || !email.contains("@")) {
            throw new IllegalArgumentException("Valid email required");
        }
    }
}

// 3. Repository Layer (Data access)
class UserRepository {
    public void save(User user) {
        // Database operations
    }
    
    public User findByEmail(String email) {
        // Database query
        return null;
    }
}

// 4. Email Service (External communication)
class EmailService {
    public void sendWelcomeEmail(String email) {
        // Email sending logic
    }
}
```

## High Cohesion, Low Coupling

### High Cohesion
**Elements within a module are closely related and focused on a single task.**

```java
// ✅ High Cohesion - All methods related to user authentication
class AuthenticationService {
    public boolean login(String username, String password) { }
    public void logout(String sessionId) { }
    public boolean validateSession(String sessionId) { }
    public void refreshToken(String token) { }
}

// ❌ Low Cohesion - Unrelated methods
class UtilityService {
    public boolean login(String username, String password) { }
    public void sendEmail(String to, String message) { }
    public double calculateTax(double amount) { }
    public void generateReport() { }
}
```

### Low Coupling
**Modules have minimal dependencies on each other.**

```java
// ❌ High Coupling - Direct dependency on concrete class
class OrderService {
    private MySQLDatabase database = new MySQLDatabase();  // Tight coupling!
    
    public void saveOrder(Order order) {
        database.save(order);
    }
}

// ✅ Low Coupling - Dependency on interface
class OrderService {
    private Database database;  // Loose coupling!
    
    public OrderService(Database database) {
        this.database = database;
    }
    
    public void saveOrder(Order order) {
        database.save(order);
    }
}
```

---

# Dependency Injection

## What is Dependency Injection?

**Providing dependencies from outside** rather than creating them inside the class.

### Without DI (Tight Coupling)

```java
// ❌ BAD - Creates its own dependencies
class UserService {
    private UserRepository repository;
    private EmailService emailService;
    
    public UserService() {
        this.repository = new UserRepository();  // Hard-coded!
        this.emailService = new EmailService();  // Hard-coded!
    }
    
    public void registerUser(User user) {
        repository.save(user);
        emailService.sendWelcome(user.getEmail());
    }
}

// Problems:
// - Can't change implementations
// - Hard to test (can't mock dependencies)
// - Violates Dependency Inversion Principle
```

### With DI (Loose Coupling)

```java
// ✅ GOOD - Dependencies injected
class UserService {
    private UserRepository repository;
    private EmailService emailService;
    
    // Constructor Injection (recommended)
    public UserService(UserRepository repository, EmailService emailService) {
        this.repository = repository;
        this.emailService = emailService;
    }
    
    public void registerUser(User user) {
        repository.save(user);
        emailService.sendWelcome(user.getEmail());
    }
}

// Usage - Dependencies provided from outside
UserRepository repo = new MySQLUserRepository();
EmailService email = new SMTPEmailService();
UserService service = new UserService(repo, email);

// Easy to switch implementations!
UserRepository mongoRepo = new MongoUserRepository();
UserService service2 = new UserService(mongoRepo, email);

// Easy to test with mocks!
UserRepository mockRepo = new MockUserRepository();
EmailService mockEmail = new MockEmailService();
UserService testService = new UserService(mockRepo, mockEmail);
```

## Types of Dependency Injection

### 1. Constructor Injection (Recommended)

```java
class OrderService {
    private final PaymentGateway gateway;
    private final OrderRepository repository;
    
    public OrderService(PaymentGateway gateway, OrderRepository repository) {
        this.gateway = gateway;
        this.repository = repository;
    }
}

// Benefits:
// - Immutable (final fields)
// - All dependencies required
// - Easy to test
```

### 2. Setter Injection

```java
class OrderService {
    private PaymentGateway gateway;
    private OrderRepository repository;
    
    public void setGateway(PaymentGateway gateway) {
        this.gateway = gateway;
    }
    
    public void setRepository(OrderRepository repository) {
        this.repository = repository;
    }
}

// Use when:
// - Optional dependencies
// - Need to change dependencies at runtime
```

### 3. Interface Injection (Rare)

```java
interface GatewayInjector {
    void injectGateway(PaymentGateway gateway);
}

class OrderService implements GatewayInjector {
    private PaymentGateway gateway;
    
    @Override
    public void injectGateway(PaymentGateway gateway) {
        this.gateway = gateway;
    }
}
```

## DI Container (Framework)

```java
// Manual DI (what we've been doing)
UserRepository repo = new MySQLUserRepository();
EmailService email = new SMTPEmailService();
UserService service = new UserService(repo, email);

// With DI Framework (e.g., Spring)
@Component
class UserService {
    private final UserRepository repository;
    private final EmailService emailService;
    
    @Autowired  // Spring automatically injects dependencies
    public UserService(UserRepository repository, EmailService emailService) {
        this.repository = repository;
        this.emailService = emailService;
    }
}

// Framework handles object creation and wiring
```

---

# Design Principles

## Composition Over Inheritance

### Problem with Inheritance

```java
// ❌ Inheritance hierarchy becomes rigid
class Animal {
    public void eat() { }
    public void sleep() { }
}

class Bird extends Animal {
    public void fly() { }
}

class Penguin extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Penguins can't fly!");
    }
}

// Problem: Penguin inherits fly() but can't use it!
```

### Solution: Composition

```java
// ✅ Use composition instead
interface Flyable {
    void fly();
}

interface Swimmable {
    void swim();
}

class FlyingBehavior implements Flyable {
    @Override
    public void fly() {
        System.out.println("Flying in the sky");
    }
}

class SwimmingBehavior implements Swimmable {
    @Override
    public void swim() {
        System.out.println("Swimming in water");
    }
}

class Bird {
    private Flyable flyBehavior;
    
    public Bird(Flyable flyBehavior) {
        this.flyBehavior = flyBehavior;
    }
    
    public void performFly() {
        flyBehavior.fly();
    }
}

class Penguin {
    private Swimmable swimBehavior;
    
    public Penguin(Swimmable swimBehavior) {
        this.swimBehavior = swimBehavior;
    }
    
    public void performSwim() {
        swimBehavior.swim();
    }
}

// Usage - Flexible!
Bird sparrow = new Bird(new FlyingBehavior());
sparrow.performFly();

Penguin penguin = new Penguin(new SwimmingBehavior());
penguin.performSwim();
```

## Program to Interfaces

```java
// ❌ BAD - Programming to implementation
class OrderProcessor {
    private ArrayList<Order> orders = new ArrayList<>();  // Concrete type
    
    public void addOrder(Order order) {
        orders.add(order);
    }
}

// ✅ GOOD - Programming to interface
class OrderProcessor {
    private List<Order> orders = new ArrayList<>();  // Interface type
    
    public void addOrder(Order order) {
        orders.add(order);
    }
    
    // Easy to change implementation
    // private List<Order> orders = new LinkedList<>();
    // private List<Order> orders = new CopyOnWriteArrayList<>();
}
```

## Favor Immutability

```java
// ❌ Mutable - Thread-unsafe, hard to reason about
class MutablePoint {
    private int x;
    private int y;
    
    public void setX(int x) { this.x = x; }
    public void setY(int y) { this.y = y; }
}

// ✅ Immutable - Thread-safe, easy to reason about
class ImmutablePoint {
    private final int x;
    private final int y;
    
    public ImmutablePoint(int x, int y) {
        this.x = x;
        this.y = y;
    }
    
    public int getX() { return x; }
    public int getY() { return y; }
    
    public ImmutablePoint move(int dx, int dy) {
        return new ImmutablePoint(x + dx, y + dy);
    }
}
```

## Fail Fast

```java
// ✅ Validate early, fail fast
class User {
    private final String email;
    
    public User(String email) {
        if (email == null || !email.contains("@")) {
            throw new IllegalArgumentException("Invalid email");
        }
        this.email = email;
    }
}

// Better than:
class User {
    private String email;
    
    public void setEmail(String email) {
        this.email = email;  // No validation - problems later!
    }
    
    public void sendEmail() {
        // Fails here - too late!
        if (email == null) {
            throw new IllegalStateException("Email not set");
        }
    }
}
```

---

## Summary

### Multithreading Key Points

1. **Thread Creation**
   - Extend Thread or implement Runnable
   - Use lambda for simple tasks

2. **Thread Safety**
   - Use `synchronized` for mutual exclusion
   - Use `volatile` for visibility
   - Prefer immutable objects

3. **Avoid Deadlocks**
   - Always acquire locks in same order
   - Use timeout for lock acquisition
   - Minimize synchronized blocks

### Architecture Key Points

1. **Separation of Concerns**
   - Controller → Service → Repository
   - Each layer has one responsibility

2. **Dependency Injection**
   - Constructor injection (preferred)
   - Depend on interfaces, not implementations
   - Makes testing easier

3. **Design Principles**
   - Composition over inheritance
   - Program to interfaces
   - High cohesion, low coupling
   - Favor immutability

### Best Practices

✅ **DO:**
- Use immutable objects when possible
- Inject dependencies via constructor
- Program to interfaces
- Keep methods small and focused
- Write tests

❌ **DON'T:**
- Create threads unnecessarily
- Use synchronized everywhere (performance)
- Create deep inheritance hierarchies
- Mix concerns in one class
- Ignore thread safety

**Remember:** Good architecture makes code easy to understand, test, and maintain!