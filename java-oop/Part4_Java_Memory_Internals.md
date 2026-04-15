# Part 4: Java Object Lifecycle & Memory - Complete Guide

## Table of Contents
1. [Java Memory Architecture](#java-memory-architecture)
2. [Object Lifecycle](#object-lifecycle)
3. [Garbage Collection](#garbage-collection)
4. [Memory Leaks in Java](#memory-leaks-in-java)
5. [equals() and hashCode()](#equals-and-hashcode)
6. [static vs instance](#static-vs-instance)
7. [Initialization Order](#initialization-order)

---

# Java Memory Architecture

## Memory Areas in JVM

```
┌─────────────────────────────────────────┐
│           JVM Memory                     │
├─────────────────────────────────────────┤
│  ┌───────────────────────────────────┐  │
│  │         HEAP                       │  │
│  │  (Objects, Instance Variables)    │  │
│  │  - Young Generation               │  │
│  │  - Old Generation                 │  │
│  │  - Shared by all threads          │  │
│  └───────────────────────────────────┘  │
│                                          │
│  ┌───────────────────────────────────┐  │
│  │         STACK                      │  │
│  │  (Method calls, Local Variables)  │  │
│  │  - One per thread                 │  │
│  │  - LIFO structure                 │  │
│  └───────────────────────────────────┘  │
│                                          │
│  ┌───────────────────────────────────┐  │
│  │      METHOD AREA                   │  │
│  │  (Class metadata, Static vars)    │  │
│  │  - Shared by all threads          │  │
│  └───────────────────────────────────┘  │
└─────────────────────────────────────────┘
```

## Heap vs Stack

### Stack Memory
- **Stores:** Method calls, local variables, references
- **Lifetime:** Exists while method is executing
- **Size:** Small (typically 1MB per thread)
- **Speed:** Very fast (LIFO)
- **Thread:** Each thread has its own stack

### Heap Memory
- **Stores:** Objects, instance variables
- **Lifetime:** Until garbage collected
- **Size:** Large (can be GBs)
- **Speed:** Slower than stack
- **Thread:** Shared by all threads

## Example: Where Things Live

```java
class Person {
    private String name;        // Heap (instance variable)
    private static int count;   // Method Area (static variable)
    
    public Person(String name) {
        this.name = name;       // 'this' reference on Stack, object on Heap
        count++;
    }
    
    public void greet() {
        String message = "Hello";  // Stack (local variable)
        System.out.println(message + ", " + name);
    }
}

public class Main {
    public static void main(String[] args) {  // Stack frame created
        Person p1 = new Person("Alice");      // p1 reference: Stack
                                               // Person object: Heap
        
        Person p2 = new Person("Bob");        // p2 reference: Stack
                                               // Person object: Heap
        
        p1.greet();  // Method call: Stack frame
    }  // Stack frame destroyed, but objects remain in Heap
}
```

### Memory Diagram

```
STACK (Thread 1)              HEAP
┌──────────────┐             ┌─────────────────────┐
│ main()       │             │ Person object       │
│  - args      │             │  name: "Alice"      │
│  - p1 ──────────────────> │  (instance vars)    │
│  - p2 ──────┐│             └─────────────────────┘
└─────────────┘│             ┌─────────────────────┐
               └──────────> │ Person object       │
                             │  name: "Bob"        │
                             │  (instance vars)    │
                             └─────────────────────┘

METHOD AREA
┌─────────────────────┐
│ Person class        │
│  - count (static)   │
│  - methods          │
└─────────────────────┘
```

---

# Object Lifecycle

## Phases of Object Life

```
1. CREATION → 2. IN USE → 3. INVISIBLE → 4. UNREACHABLE → 5. COLLECTED → 6. FINALIZED → 7. DEALLOCATED
```

## 1. Object Creation

```java
class Student {
    private String name;
    private int age;
    
    // Constructor
    public Student(String name, int age) {
        System.out.println("Creating student: " + name);
        this.name = name;
        this.age = age;
    }
}

// Creation happens here
Student student = new Student("John", 20);
```

**What happens:**
1. Memory allocated on Heap
2. Instance variables initialized to default values
3. Constructor executed
4. Reference returned and stored in Stack

## 2. In Use (Reachable)

```java
Student student = new Student("John", 20);
student.getName();  // Object is reachable and in use
```

Object is **reachable** if there's a reference path from:
- Local variables
- Static variables
- Active threads

## 3. Invisible

```java
public void method() {
    Student student = new Student("John", 20);
    
    if (true) {
        Student temp = new Student("Jane", 21);
        // temp is visible here
    }
    // temp is invisible here (out of scope)
    // but object still exists in memory!
}
```

## 4. Unreachable (Eligible for GC)

```java
Student student = new Student("John", 20);
student = null;  // Object becomes unreachable
// OR
student = new Student("Jane", 21);  // Old object becomes unreachable
```

## 5. Garbage Collection

JVM's Garbage Collector automatically reclaims memory.

## 6. Finalized

```java
class Resource {
    @Override
    protected void finalize() throws Throwable {
        System.out.println("Finalizing resource");
        // Cleanup code (not recommended - use try-with-resources instead)
    }
}
```

## 7. Deallocated

Memory returned to heap for reuse.

---

# Garbage Collection

## What is Garbage Collection?

**Automatic memory management** - JVM automatically frees memory of unreachable objects.

### Benefits
- ✅ No manual memory management
- ✅ Prevents memory leaks (mostly)
- ✅ Prevents dangling pointers

### Drawbacks
- ❌ Performance overhead
- ❌ Unpredictable timing
- ❌ Can cause pauses

## How GC Works

### Reachability

```java
class GCDemo {
    public static void main(String[] args) {
        // Object 1 - Reachable (referenced by obj1)
        String obj1 = new String("Hello");
        
        // Object 2 - Reachable (referenced by obj2)
        String obj2 = new String("World");
        
        // Object 3 - Unreachable (no reference)
        new String("Garbage");  // Eligible for GC immediately!
        
        obj1 = null;  // Object 1 now unreachable - eligible for GC
        
        // Object 2 still reachable
        System.out.println(obj2);
    }
}
```

### Reachability Types

```java
class ReachabilityDemo {
    static Object staticRef;           // Strong reference
    
    public static void main(String[] args) {
        // 1. Strong Reference (default)
        Object strong = new Object();  // Won't be GC'd while reference exists
        
        // 2. Weak Reference
        WeakReference<Object> weak = new WeakReference<>(new Object());
        // Can be GC'd even if weak reference exists
        
        // 3. Soft Reference
        SoftReference<Object> soft = new SoftReference<>(new Object());
        // GC'd only if memory is low
        
        // 4. Phantom Reference
        PhantomReference<Object> phantom = new PhantomReference<>(
            new Object(), 
            new ReferenceQueue<>()
        );
        // Used for cleanup actions
    }
}
```

## Generational Garbage Collection

```
HEAP
┌────────────────────────────────────────┐
│  YOUNG GENERATION                      │
│  ┌──────────┬──────────┬──────────┐   │
│  │  Eden    │ Survivor │ Survivor │   │
│  │  Space   │    S0    │    S1    │   │
│  │          │          │          │   │
│  │ New      │ Survived │ Survived │   │
│  │ objects  │ 1 GC     │ 2+ GCs   │   │
│  └──────────┴──────────┴──────────┘   │
│                                        │
│  OLD GENERATION (Tenured)              │
│  ┌────────────────────────────────┐   │
│  │                                │   │
│  │  Long-lived objects            │   │
│  │  (survived many GCs)           │   │
│  │                                │   │
│  └────────────────────────────────┘   │
└────────────────────────────────────────┘
```

### Process

1. **New objects** → Eden Space
2. **Minor GC** → Survivors move to S0
3. **Next Minor GC** → Survivors move to S1
4. **After many GCs** → Move to Old Generation
5. **Major GC** → Clean Old Generation (expensive!)

## Forcing Garbage Collection

```java
class GCExample {
    public static void main(String[] args) {
        // Create objects
        for (int i = 0; i < 1000; i++) {
            String temp = new String("Object " + i);
        }
        
        // Suggest GC (not guaranteed!)
        System.gc();  // or Runtime.getRuntime().gc();
        
        // Better: Let JVM decide when to run GC
    }
}
```

**Note:** `System.gc()` is just a suggestion. JVM may ignore it!

---

# Memory Leaks in Java

## What is a Memory Leak?

**Objects that are no longer needed but still reachable**, preventing garbage collection.

## Common Causes

### 1. Static Collections

```java
// ❌ BAD - Memory Leak!
class UserCache {
    private static List<User> cache = new ArrayList<>();
    
    public void addUser(User user) {
        cache.add(user);  // Never removed - grows forever!
    }
}

// ✅ GOOD - Use bounded cache
class UserCache {
    private static Map<String, User> cache = new LinkedHashMap<String, User>(100, 0.75f, true) {
        @Override
        protected boolean removeEldestEntry(Map.Entry eldest) {
            return size() > 100;  // Limit size
        }
    };
}
```

### 2. Unclosed Resources

```java
// ❌ BAD - Resource leak!
class FileProcessor {
    public void process(String filename) {
        FileInputStream fis = new FileInputStream(filename);
        // Process file
        // fis never closed - leak!
    }
}

// ✅ GOOD - Use try-with-resources
class FileProcessor {
    public void process(String filename) {
        try (FileInputStream fis = new FileInputStream(filename)) {
            // Process file
        } catch (IOException e) {
            e.printStackTrace();
        }
        // fis automatically closed
    }
}
```

### 3. Listeners Not Removed

```java
// ❌ BAD - Listener leak!
class EventSource {
    private List<EventListener> listeners = new ArrayList<>();
    
    public void addListener(EventListener listener) {
        listeners.add(listener);
    }
    
    // Missing: removeListener method!
}

// ✅ GOOD - Provide removal
class EventSource {
    private List<EventListener> listeners = new ArrayList<>();
    
    public void addListener(EventListener listener) {
        listeners.add(listener);
    }
    
    public void removeListener(EventListener listener) {
        listeners.remove(listener);
    }
}
```

### 4. Inner Class References

```java
// ❌ BAD - Inner class holds reference to outer class
class OuterClass {
    private byte[] data = new byte[1000000];  // 1MB
    
    class InnerClass {
        public void doSomething() {
            // Implicitly holds reference to OuterClass
            // Even if OuterClass is done, it can't be GC'd!
        }
    }
    
    public InnerClass getInner() {
        return new InnerClass();
    }
}

// ✅ GOOD - Use static inner class
class OuterClass {
    private byte[] data = new byte[1000000];
    
    static class InnerClass {
        public void doSomething() {
            // No implicit reference to OuterClass
        }
    }
    
    public InnerClass getInner() {
        return new InnerClass();
    }
}
```

### 5. ThreadLocal Variables

```java
// ❌ BAD - ThreadLocal leak!
class UserContext {
    private static ThreadLocal<User> currentUser = new ThreadLocal<>();
    
    public static void setUser(User user) {
        currentUser.set(user);
        // Never removed - leak in thread pools!
    }
}

// ✅ GOOD - Always remove
class UserContext {
    private static ThreadLocal<User> currentUser = new ThreadLocal<>();
    
    public static void setUser(User user) {
        currentUser.set(user);
    }
    
    public static void clear() {
        currentUser.remove();  // Important!
    }
}

// Usage
try {
    UserContext.setUser(user);
    // Do work
} finally {
    UserContext.clear();  // Always clean up
}
```

## Detecting Memory Leaks

### 1. Monitor Heap Usage

```java
Runtime runtime = Runtime.getRuntime();
long totalMemory = runtime.totalMemory();
long freeMemory = runtime.freeMemory();
long usedMemory = totalMemory - freeMemory;

System.out.println("Used Memory: " + usedMemory / (1024 * 1024) + " MB");
```

### 2. Use Profilers
- VisualVM
- JProfiler
- YourKit
- Eclipse MAT (Memory Analyzer Tool)

### 3. Heap Dump Analysis

```bash
# Generate heap dump
jmap -dump:format=b,file=heap.bin <pid>

# Analyze with MAT or VisualVM
```

---

# equals() and hashCode()

## Why Override These Methods?

```java
class Person {
    private String name;
    private int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

// Without overriding equals()
Person p1 = new Person("John", 30);
Person p2 = new Person("John", 30);

System.out.println(p1 == p2);        // false (different objects)
System.out.println(p1.equals(p2));   // false (uses Object.equals - reference comparison)

// With HashMap
Map<Person, String> map = new HashMap<>();
map.put(p1, "Engineer");
System.out.println(map.get(p2));     // null (different hash codes!)
```

## The Contract

### equals() Contract
1. **Reflexive:** `x.equals(x)` must be true
2. **Symmetric:** If `x.equals(y)`, then `y.equals(x)`
3. **Transitive:** If `x.equals(y)` and `y.equals(z)`, then `x.equals(z)`
4. **Consistent:** Multiple calls return same result
5. **Null:** `x.equals(null)` must be false

### hashCode() Contract
1. **Consistent:** Same object → same hash code
2. **Equal objects:** If `x.equals(y)`, then `x.hashCode() == y.hashCode()`
3. **Unequal objects:** Different objects CAN have same hash code (collision)

## ✅ Correct Implementation

```java
class Person {
    private String name;
    private int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    @Override
    public boolean equals(Object obj) {
        // 1. Check if same object
        if (this == obj) return true;
        
        // 2. Check if null
        if (obj == null) return false;
        
        // 3. Check if same class
        if (getClass() != obj.getClass()) return false;
        
        // 4. Cast and compare fields
        Person other = (Person) obj;
        return age == other.age && 
               Objects.equals(name, other.name);
    }
    
    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
}

// Now it works!
Person p1 = new Person("John", 30);
Person p2 = new Person("John", 30);

System.out.println(p1.equals(p2));   // true

Map<Person, String> map = new HashMap<>();
map.put(p1, "Engineer");
System.out.println(map.get(p2));     // "Engineer" ✓
```

## Common Mistakes

### ❌ Mistake 1: Override equals() but not hashCode()

```java
class Person {
    private String name;
    
    @Override
    public boolean equals(Object obj) {
        if (obj instanceof Person) {
            return name.equals(((Person) obj).name);
        }
        return false;
    }
    
    // Missing hashCode()!
}

// Problem:
Person p1 = new Person("John");
Person p2 = new Person("John");

Set<Person> set = new HashSet<>();
set.add(p1);
set.add(p2);  // Both added! (different hash codes)
System.out.println(set.size());  // 2 (should be 1!)
```

### ❌ Mistake 2: Using mutable fields

```java
class Person {
    private String name;  // Mutable!
    
    public void setName(String name) {
        this.name = name;
    }
    
    @Override
    public boolean equals(Object obj) {
        return name.equals(((Person) obj).name);
    }
    
    @Override
    public int hashCode() {
        return name.hashCode();
    }
}

// Problem:
Person p = new Person("John");
Set<Person> set = new HashSet<>();
set.add(p);

p.setName("Jane");  // Hash code changes!
System.out.println(set.contains(p));  // false! Lost in HashSet!
```

**Solution:** Use immutable fields or don't use mutable objects as keys.

---

# static vs instance

## Static Members

```java
class Counter {
    // Static variable - shared by ALL instances
    private static int count = 0;
    
    // Instance variable - unique per instance
    private int id;
    
    public Counter() {
        count++;  // Increment shared counter
        id = count;  // Assign unique ID
    }
    
    // Static method - belongs to class
    public static int getCount() {
        return count;
        // Cannot access 'id' here - it's instance-specific!
    }
    
    // Instance method - belongs to object
    public int getId() {
        return id;
        // Can access both 'id' and 'count'
    }
}

// Usage
Counter c1 = new Counter();
Counter c2 = new Counter();
Counter c3 = new Counter();

System.out.println(Counter.getCount());  // 3 (static method)
System.out.println(c1.getId());          // 1
System.out.println(c2.getId());          // 2
System.out.println(c3.getId());          // 3
```

## Memory Diagram

```
METHOD AREA
┌─────────────────────┐
│ Counter class       │
│  - count = 3        │ ← Static variable (shared)
│  - getCount()       │ ← Static method
└─────────────────────┘

HEAP
┌─────────────────┐
│ Counter object  │
│  - id = 1       │ ← Instance variable
└─────────────────┘

┌─────────────────┐
│ Counter object  │
│  - id = 2       │ ← Instance variable
└─────────────────┘

┌─────────────────┐
│ Counter object  │
│  - id = 3       │ ← Instance variable
└─────────────────┘
```

## Static vs Instance Comparison

| Feature | Static | Instance |
|---------|--------|----------|
| **Belongs to** | Class | Object |
| **Memory** | Method Area | Heap |
| **Access** | ClassName.member | object.member |
| **Shared** | Yes (all instances) | No (per instance) |
| **Can access** | Only static members | Both static and instance |
| **When loaded** | Class loading | Object creation |

## Static Block

```java
class DatabaseConfig {
    private static String url;
    private static String username;
    
    // Static block - runs once when class is loaded
    static {
        System.out.println("Loading database configuration...");
        url = "jdbc:mysql://localhost:3306/mydb";
        username = "admin";
    }
    
    public static String getUrl() {
        return url;
    }
}

// First access triggers class loading
System.out.println(DatabaseConfig.getUrl());
// Output:
// Loading database configuration...
// jdbc:mysql://localhost:3306/mydb
```

---

# Initialization Order

## Complete Initialization Sequence

```java
class Parent {
    // 1. Static variables
    private static String staticVar = initStaticVar();
    
    // 2. Static block
    static {
        System.out.println("2. Parent static block");
    }
    
    // 3. Instance variables
    private String instanceVar = initInstanceVar();
    
    // 4. Instance block
    {
        System.out.println("4. Parent instance block");
    }
    
    // 5. Constructor
    public Parent() {
        System.out.println("5. Parent constructor");
    }
    
    private static String initStaticVar() {
        System.out.println("1. Parent static variable");
        return "static";
    }
    
    private String initInstanceVar() {
        System.out.println("3. Parent instance variable");
        return "instance";
    }
}

class Child extends Parent {
    // 6. Static variables
    private static String staticVar = initStaticVar();
    
    // 7. Static block
    static {
        System.out.println("7. Child static block");
    }
    
    // 8. Instance variables
    private String instanceVar = initInstanceVar();
    
    // 9. Instance block
    {
        System.out.println("9. Child instance block");
    }
    
    // 10. Constructor
    public Child() {
        System.out.println("10. Child constructor");
    }
    
    private static String initStaticVar() {
        System.out.println("6. Child static variable");
        return "static";
    }
    
    private String initInstanceVar() {
        System.out.println("8. Child instance variable");
        return "instance";
    }
}

// Usage
Child child = new Child();

// Output:
// 1. Parent static variable
// 2. Parent static block
// 6. Child static variable
// 7. Child static block
// 3. Parent instance variable
// 4. Parent instance block
// 5. Parent constructor
// 8. Child instance variable
// 9. Child instance block
// 10. Child constructor
```

## Initialization Order Rules

```
CLASS LOADING (once per class)
1. Parent static variables
2. Parent static blocks
3. Child static variables
4. Child static blocks

OBJECT CREATION (per instance)
5. Parent instance variables
6. Parent instance blocks
7. Parent constructor
8. Child instance variables
9. Child instance blocks
10. Child constructor
```

## Practical Example

```java
class Configuration {
    // 1. Static - loaded once
    private static final String APP_NAME = "MyApp";
    
    static {
        System.out.println("Loading " + APP_NAME);
    }
    
    // 2. Instance - per object
    private String userId;
    
    {
        System.out.println("Creating configuration instance");
    }
    
    // 3. Constructor
    public Configuration(String userId) {
        this.userId = userId;
        System.out.println("Configuration for user: " + userId);
    }
}

// First object
Configuration config1 = new Configuration("user1");
// Output:
// Loading MyApp
// Creating configuration instance
// Configuration for user: user1

// Second object
Configuration config2 = new Configuration("user2");
// Output:
// Creating configuration instance
// Configuration for user: user2
// (Static block doesn't run again!)
```

---

## Summary

### Key Takeaways

1. **Memory Management**
   - Stack: Fast, small, method-local
   - Heap: Slower, large, objects
   - Method Area: Class metadata, static

2. **Garbage Collection**
   - Automatic memory management
   - Based on reachability
   - Generational approach

3. **Memory Leaks**
   - Static collections
   - Unclosed resources
   - Listeners not removed
   - ThreadLocal not cleared

4. **equals() and hashCode()**
   - Always override both together
   - Use immutable fields
   - Follow the contract

5. **static vs instance**
   - Static: Class-level, shared
   - Instance: Object-level, unique

6. **Initialization Order**
   - Static first (parent → child)
   - Then instance (parent → child)
   - Finally constructors (parent → child)

### Best Practices

✅ **DO:**
- Use try-with-resources for auto-closing
- Override equals() and hashCode() together
- Remove listeners when done
- Clear ThreadLocal variables
- Let JVM handle GC (don't call System.gc())

❌ **DON'T:**
- Store everything in static collections
- Use mutable objects as HashMap keys
- Forget to close resources
- Rely on finalize() (deprecated)
- Create memory leaks with inner classes

**Remember:** Understanding memory management helps you write efficient, leak-free Java code!