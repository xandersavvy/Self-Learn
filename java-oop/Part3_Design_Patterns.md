# Part 3: Design Patterns - Complete Guide

## Table of Contents
1. [Introduction to Design Patterns](#introduction-to-design-patterns)
2. [Creational Patterns](#creational-patterns)
   - Singleton
   - Factory Method
   - Abstract Factory
   - Builder
3. [Structural Patterns](#structural-patterns)
   - Adapter
   - Bridge
   - Decorator
   - Facade
4. [Behavioral Patterns](#behavioral-patterns)
   - Strategy
   - Observer
   - Command
   - Interpreter
5. [Pattern Recognition Guide](#pattern-recognition-guide)

---

# Introduction to Design Patterns

## What are Design Patterns?
**Design patterns are reusable solutions to common problems in software design.** They're like blueprints you can customize to solve recurring design problems in your code.

## Why Learn Design Patterns?
- ✅ Proven solutions to common problems
- ✅ Common vocabulary for developers
- ✅ Speed up development
- ✅ Make code more maintainable
- ✅ Improve code quality

## Three Categories
1. **Creational** - How objects are created
2. **Structural** - How objects are composed
3. **Behavioral** - How objects interact

---

# Creational Patterns

Creational patterns deal with object creation mechanisms.

---

## 1. Singleton Pattern

### Problem
You need **exactly ONE instance** of a class throughout your application.

### Real-World Analogy
- A country has ONE president
- A company has ONE CEO
- Your computer has ONE operating system instance

### When to Use
- Database connections
- Configuration managers
- Logging services
- Thread pools

### ❌ Problem Without Singleton

```java
class DatabaseConnection {
    public DatabaseConnection() {
        System.out.println("Creating new database connection...");
        // Expensive operation!
    }
    
    public void query(String sql) {
        System.out.println("Executing: " + sql);
    }
}

// Problem: Multiple instances created!
DatabaseConnection db1 = new DatabaseConnection();
DatabaseConnection db2 = new DatabaseConnection();
DatabaseConnection db3 = new DatabaseConnection();
// Output:
// Creating new database connection...
// Creating new database connection...
// Creating new database connection...
```

### ✅ Solution: Singleton Pattern

```java
class DatabaseConnection {
    // 1. Private static instance
    private static DatabaseConnection instance;
    
    // 2. Private constructor - prevents external instantiation
    private DatabaseConnection() {
        System.out.println("Creating database connection...");
    }
    
    // 3. Public static method to get instance
    public static DatabaseConnection getInstance() {
        if (instance == null) {
            instance = new DatabaseConnection();
        }
        return instance;
    }
    
    public void query(String sql) {
        System.out.println("Executing: " + sql);
    }
}

// Usage
DatabaseConnection db1 = DatabaseConnection.getInstance();
DatabaseConnection db2 = DatabaseConnection.getInstance();
DatabaseConnection db3 = DatabaseConnection.getInstance();

System.out.println(db1 == db2);  // true - same instance!
System.out.println(db2 == db3);  // true - same instance!
```

### Thread-Safe Singleton (Important!)

```java
// Method 1: Eager Initialization (Thread-safe)
class ConfigManager {
    private static final ConfigManager instance = new ConfigManager();
    
    private ConfigManager() {
        System.out.println("Loading configuration...");
    }
    
    public static ConfigManager getInstance() {
        return instance;
    }
}

// Method 2: Double-Checked Locking (Lazy + Thread-safe)
class Logger {
    private static volatile Logger instance;
    
    private Logger() {
        System.out.println("Initializing logger...");
    }
    
    public static Logger getInstance() {
        if (instance == null) {  // First check (no locking)
            synchronized (Logger.class) {
                if (instance == null) {  // Second check (with locking)
                    instance = new Logger();
                }
            }
        }
        return instance;
    }
    
    public void log(String message) {
        System.out.println("[LOG] " + message);
    }
}

// Method 3: Bill Pugh Singleton (Best approach)
class AppSettings {
    private AppSettings() {
        System.out.println("Loading settings...");
    }
    
    // Inner static class - loaded only when accessed
    private static class SingletonHelper {
        private static final AppSettings INSTANCE = new AppSettings();
    }
    
    public static AppSettings getInstance() {
        return SingletonHelper.INSTANCE;
    }
}
```

### Problems with Singleton
- ❌ Hard to test (global state)
- ❌ Violates Single Responsibility Principle
- ❌ Can be broken by reflection or serialization
- ❌ Hidden dependencies

**Better Alternative:** Dependency Injection

---

## 2. Factory Method Pattern

### Problem
You need to create objects without specifying their exact classes.

### Real-World Analogy
A restaurant menu:
- You order "Pizza" (interface)
- Kitchen decides whether to make Margherita, Pepperoni, or Veggie (concrete classes)

### When to Use
- When you don't know exact types beforehand
- When creation logic is complex
- When you want to delegate object creation

### ❌ Problem Without Factory

```java
// Client code tightly coupled to concrete classes
class ShapeClient {
    public void drawShape(String type) {
        Shape shape;
        
        if (type.equals("circle")) {
            shape = new Circle();
        } else if (type.equals("rectangle")) {
            shape = new Rectangle();
        } else if (type.equals("triangle")) {
            shape = new Triangle();
        } else {
            throw new IllegalArgumentException("Unknown shape");
        }
        
        shape.draw();
    }
}
```

### ✅ Solution: Factory Method

```java
// Product interface
interface Shape {
    void draw();
}

// Concrete products
class Circle implements Shape {
    @Override
    public void draw() {
        System.out.println("Drawing a Circle");
    }
}

class Rectangle implements Shape {
    @Override
    public void draw() {
        System.out.println("Drawing a Rectangle");
    }
}

class Triangle implements Shape {
    @Override
    public void draw() {
        System.out.println("Drawing a Triangle");
    }
}

// Factory class
class ShapeFactory {
    // Factory method
    public static Shape createShape(String type) {
        switch (type.toLowerCase()) {
            case "circle":
                return new Circle();
            case "rectangle":
                return new Rectangle();
            case "triangle":
                return new Triangle();
            default:
                throw new IllegalArgumentException("Unknown shape: " + type);
        }
    }
}

// Usage
Shape shape1 = ShapeFactory.createShape("circle");
shape1.draw();

Shape shape2 = ShapeFactory.createShape("rectangle");
shape2.draw();
```

### Real-World Example: Document Creator

```java
// Product interface
interface Document {
    void open();
    void save();
}

// Concrete products
class PDFDocument implements Document {
    @Override
    public void open() {
        System.out.println("Opening PDF document");
    }
    
    @Override
    public void save() {
        System.out.println("Saving as PDF");
    }
}

class WordDocument implements Document {
    @Override
    public void open() {
        System.out.println("Opening Word document");
    }
    
    @Override
    public void save() {
        System.out.println("Saving as DOCX");
    }
}

class ExcelDocument implements Document {
    @Override
    public void open() {
        System.out.println("Opening Excel spreadsheet");
    }
    
    @Override
    public void save() {
        System.out.println("Saving as XLSX");
    }
}

// Factory
class DocumentFactory {
    public static Document createDocument(String type) {
        switch (type.toLowerCase()) {
            case "pdf":
                return new PDFDocument();
            case "word":
                return new WordDocument();
            case "excel":
                return new ExcelDocument();
            default:
                throw new IllegalArgumentException("Unknown document type");
        }
    }
}

// Usage
Document doc1 = DocumentFactory.createDocument("pdf");
doc1.open();
doc1.save();

Document doc2 = DocumentFactory.createDocument("word");
doc2.open();
doc2.save();
```

---

## 3. Abstract Factory Pattern

### Problem
You need to create **families of related objects** without specifying their concrete classes.

### Real-World Analogy
A furniture store:
- Modern style: Modern Chair + Modern Table + Modern Sofa
- Victorian style: Victorian Chair + Victorian Table + Victorian Sofa

### When to Use
- When you need to create families of related objects
- When you want to ensure objects are compatible
- When you want to hide implementation details

### ✅ Abstract Factory Implementation

```java
// Abstract products
interface Button {
    void render();
}

interface Checkbox {
    void render();
}

// Concrete products - Windows style
class WindowsButton implements Button {
    @Override
    public void render() {
        System.out.println("Rendering Windows-style button");
    }
}

class WindowsCheckbox implements Checkbox {
    @Override
    public void render() {
        System.out.println("Rendering Windows-style checkbox");
    }
}

// Concrete products - Mac style
class MacButton implements Button {
    @Override
    public void render() {
        System.out.println("Rendering Mac-style button");
    }
}

class MacCheckbox implements Checkbox {
    @Override
    public void render() {
        System.out.println("Rendering Mac-style checkbox");
    }
}

// Abstract factory
interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}

// Concrete factories
class WindowsFactory implements GUIFactory {
    @Override
    public Button createButton() {
        return new WindowsButton();
    }
    
    @Override
    public Checkbox createCheckbox() {
        return new WindowsCheckbox();
    }
}

class MacFactory implements GUIFactory {
    @Override
    public Button createButton() {
        return new MacButton();
    }
    
    @Override
    public Checkbox createCheckbox() {
        return new MacCheckbox();
    }
}

// Client code
class Application {
    private Button button;
    private Checkbox checkbox;
    
    public Application(GUIFactory factory) {
        button = factory.createButton();
        checkbox = factory.createCheckbox();
    }
    
    public void render() {
        button.render();
        checkbox.render();
    }
}

// Usage
String os = System.getProperty("os.name").toLowerCase();
GUIFactory factory;

if (os.contains("win")) {
    factory = new WindowsFactory();
} else if (os.contains("mac")) {
    factory = new MacFactory();
} else {
    factory = new WindowsFactory();  // Default
}

Application app = new Application(factory);
app.render();
```

---

## 4. Builder Pattern

### Problem
You need to construct **complex objects step by step**, especially when there are many optional parameters.

### Real-World Analogy
Building a custom pizza:
- Choose size
- Choose crust
- Add toppings
- Add extra cheese
- Choose sauce

### When to Use
- Many constructor parameters (especially optional ones)
- Object creation is complex
- Want immutable objects with many fields

### ❌ Problem: Telescoping Constructor

```java
class Pizza {
    private String size;
    private boolean cheese;
    private boolean pepperoni;
    private boolean bacon;
    private boolean mushrooms;
    private boolean onions;
    
    // Constructor hell!
    public Pizza(String size) { }
    public Pizza(String size, boolean cheese) { }
    public Pizza(String size, boolean cheese, boolean pepperoni) { }
    public Pizza(String size, boolean cheese, boolean pepperoni, boolean bacon) { }
    // ... many more constructors!
}
```

### ✅ Solution: Builder Pattern

```java
class Pizza {
    // Required parameters
    private final String size;
    
    // Optional parameters
    private final boolean cheese;
    private final boolean pepperoni;
    private final boolean bacon;
    private final boolean mushrooms;
    private final boolean onions;
    
    // Private constructor
    private Pizza(PizzaBuilder builder) {
        this.size = builder.size;
        this.cheese = builder.cheese;
        this.pepperoni = builder.pepperoni;
        this.bacon = builder.bacon;
        this.mushrooms = builder.mushrooms;
        this.onions = builder.onions;
    }
    
    @Override
    public String toString() {
        return "Pizza [size=" + size + 
               ", cheese=" + cheese + 
               ", pepperoni=" + pepperoni + 
               ", bacon=" + bacon + 
               ", mushrooms=" + mushrooms + 
               ", onions=" + onions + "]";
    }
    
    // Static nested Builder class
    public static class PizzaBuilder {
        // Required parameters
        private final String size;
        
        // Optional parameters - initialized to default values
        private boolean cheese = false;
        private boolean pepperoni = false;
        private boolean bacon = false;
        private boolean mushrooms = false;
        private boolean onions = false;
        
        public PizzaBuilder(String size) {
            this.size = size;
        }
        
        public PizzaBuilder cheese(boolean value) {
            cheese = value;
            return this;
        }
        
        public PizzaBuilder pepperoni(boolean value) {
            pepperoni = value;
            return this;
        }
        
        public PizzaBuilder bacon(boolean value) {
            bacon = value;
            return this;
        }
        
        public PizzaBuilder mushrooms(boolean value) {
            mushrooms = value;
            return this;
        }
        
        public PizzaBuilder onions(boolean value) {
            onions = value;
            return this;
        }
        
        public Pizza build() {
            return new Pizza(this);
        }
    }
}

// Usage - Fluent interface!
Pizza pizza1 = new Pizza.PizzaBuilder("Large")
    .cheese(true)
    .pepperoni(true)
    .bacon(true)
    .build();

Pizza pizza2 = new Pizza.PizzaBuilder("Medium")
    .cheese(true)
    .mushrooms(true)
    .onions(true)
    .build();

Pizza pizza3 = new Pizza.PizzaBuilder("Small")
    .cheese(true)
    .build();

System.out.println(pizza1);
System.out.println(pizza2);
System.out.println(pizza3);
```

### Real-World Example: HTTP Request Builder

```java
class HttpRequest {
    private final String url;
    private final String method;
    private final Map<String, String> headers;
    private final String body;
    private final int timeout;
    
    private HttpRequest(Builder builder) {
        this.url = builder.url;
        this.method = builder.method;
        this.headers = builder.headers;
        this.body = builder.body;
        this.timeout = builder.timeout;
    }
    
    public void send() {
        System.out.println("Sending " + method + " request to " + url);
        System.out.println("Headers: " + headers);
        System.out.println("Body: " + body);
        System.out.println("Timeout: " + timeout + "ms");
    }
    
    public static class Builder {
        private final String url;
        private String method = "GET";
        private Map<String, String> headers = new HashMap<>();
        private String body = "";
        private int timeout = 5000;
        
        public Builder(String url) {
            this.url = url;
        }
        
        public Builder method(String method) {
            this.method = method;
            return this;
        }
        
        public Builder header(String key, String value) {
            this.headers.put(key, value);
            return this;
        }
        
        public Builder body(String body) {
            this.body = body;
            return this;
        }
        
        public Builder timeout(int timeout) {
            this.timeout = timeout;
            return this;
        }
        
        public HttpRequest build() {
            return new HttpRequest(this);
        }
    }
}

// Usage
HttpRequest request = new HttpRequest.Builder("https://api.example.com/users")
    .method("POST")
    .header("Content-Type", "application/json")
    .header("Authorization", "Bearer token123")
    .body("{\"name\": \"John\", \"age\": 30}")
    .timeout(10000)
    .build();

request.send();
```

---

# Structural Patterns

Structural patterns deal with object composition and relationships.

---

## 5. Adapter Pattern

### Problem
You need to make **incompatible interfaces work together**.

### Real-World Analogy
- Power adapter: US plug → European socket
- Language translator: English → Spanish
- Card reader: SD card → USB port

### When to Use
- Legacy code integration
- Third-party library integration
- Interface mismatch

### ✅ Adapter Implementation

```java
// Target interface (what client expects)
interface MediaPlayer {
    void play(String filename);
}

// Adaptee (incompatible interface)
class AdvancedMediaPlayer {
    public void playVlc(String filename) {
        System.out.println("Playing VLC file: " + filename);
    }
    
    public void playMp4(String filename) {
        System.out.println("Playing MP4 file: " + filename);
    }
}

// Adapter (makes AdvancedMediaPlayer compatible with MediaPlayer)
class MediaAdapter implements MediaPlayer {
    private AdvancedMediaPlayer advancedPlayer;
    
    public MediaAdapter() {
        this.advancedPlayer = new AdvancedMediaPlayer();
    }
    
    @Override
    public void play(String filename) {
        if (filename.endsWith(".vlc")) {
            advancedPlayer.playVlc(filename);
        } else if (filename.endsWith(".mp4")) {
            advancedPlayer.playMp4(filename);
        } else {
            System.out.println("Unsupported format");
        }
    }
}

// Client
class AudioPlayer implements MediaPlayer {
    private MediaAdapter adapter;
    
    @Override
    public void play(String filename) {
        if (filename.endsWith(".mp3")) {
            System.out.println("Playing MP3 file: " + filename);
        } else if (filename.endsWith(".vlc") || filename.endsWith(".mp4")) {
            adapter = new MediaAdapter();
            adapter.play(filename);
        } else {
            System.out.println("Invalid format");
        }
    }
}

// Usage
MediaPlayer player = new AudioPlayer();
player.play("song.mp3");
player.play("movie.mp4");
player.play("video.vlc");
```

---

## 6. Bridge Pattern

### Problem
You need to **separate abstraction from implementation** so both can vary independently.

### Real-World Analogy
Remote control (abstraction) works with any TV (implementation):
- Sony Remote → Sony TV
- Sony Remote → Samsung TV
- Universal Remote → Any TV

### When to Use
- Avoid permanent binding between abstraction and implementation
- Both abstraction and implementation should be extensible
- Changes in implementation shouldn't affect clients

### Classic Example: Shape and Color

```java
// Implementation interface
interface Color {
    void applyColor();
}

// Concrete implementations
class Red implements Color {
    @Override
    public void applyColor() {
        System.out.println("Applying red color");
    }
}

class Blue implements Color {
    @Override
    public void applyColor() {
        System.out.println("Applying blue color");
    }
}

class Green implements Color {
    @Override
    public void applyColor() {
        System.out.println("Applying green color");
    }
}

// Abstraction
abstract class Shape {
    protected Color color;  // Bridge to implementation
    
    public Shape(Color color) {
        this.color = color;
    }
    
    abstract void draw();
}

// Refined abstractions
class Circle extends Shape {
    public Circle(Color color) {
        super(color);
    }
    
    @Override
    void draw() {
        System.out.print("Drawing Circle. ");
        color.applyColor();
    }
}

class Rectangle extends Shape {
    public Rectangle(Color color) {
        super(color);
    }
    
    @Override
    void draw() {
        System.out.print("Drawing Rectangle. ");
        color.applyColor();
    }
}

class Triangle extends Shape {
    public Triangle(Color color) {
        super(color);
    }
    
    @Override
    void draw() {
        System.out.print("Drawing Triangle. ");
        color.applyColor();
    }
}

// Usage - Mix and match!
Shape redCircle = new Circle(new Red());
Shape blueRectangle = new Rectangle(new Blue());
Shape greenTriangle = new Triangle(new Green());

redCircle.draw();
blueRectangle.draw();
greenTriangle.draw();

// Easy to add new shapes or colors without modifying existing code!
```

### Real-World Example: Device and Remote

```java
// Implementation
interface Device {
    void turnOn();
    void turnOff();
    void setVolume(int volume);
}

class TV implements Device {
    private int volume = 50;
    
    @Override
    public void turnOn() {
        System.out.println("TV is ON");
    }
    
    @Override
    public void turnOff() {
        System.out.println("TV is OFF");
    }
    
    @Override
    public void setVolume(int volume) {
        this.volume = volume;
        System.out.println("TV volume set to " + volume);
    }
}

class Radio implements Device {
    private int volume = 30;
    
    @Override
    public void turnOn() {
        System.out.println("Radio is ON");
    }
    
    @Override
    public void turnOff() {
        System.out.println("Radio is OFF");
    }
    
    @Override
    public void setVolume(int volume) {
        this.volume = volume;
        System.out.println("Radio volume set to " + volume);
    }
}

// Abstraction
class RemoteControl {
    protected Device device;
    
    public RemoteControl(Device device) {
        this.device = device;
    }
    
    public void togglePower() {
        System.out.println("Toggling power...");
        device.turnOn();
    }
    
    public void volumeUp() {
        System.out.println("Volume up");
        device.setVolume(60);
    }
}

// Refined abstraction
class AdvancedRemote extends RemoteControl {
    public AdvancedRemote(Device device) {
        super(device);
    }
    
    public void mute() {
        System.out.println("Muting device");
        device.setVolume(0);
    }
}

// Usage
Device tv = new TV();
RemoteControl remote1 = new RemoteControl(tv);
remote1.togglePower();
remote1.volumeUp();

Device radio = new Radio();
AdvancedRemote remote2 = new AdvancedRemote(radio);
remote2.togglePower();
remote2.mute();
```

---

## 7. Decorator Pattern

### Problem
You need to **add new functionality to objects dynamically** without altering their structure.

### Real-World Analogy
Coffee shop:
- Basic coffee
- + Milk = Coffee with milk
- + Milk + Sugar = Coffee with milk and sugar
- + Milk + Sugar + Whipped cream = Fancy coffee

### When to Use
- Add responsibilities to objects dynamically
- Avoid subclass explosion
- Extend functionality without modifying code

### ✅ Decorator Implementation

```java
// Component interface
interface Coffee {
    String getDescription();
    double getCost();
}

// Concrete component
class SimpleCoffee implements Coffee {
    @Override
    public String getDescription() {
        return "Simple Coffee";
    }
    
    @Override
    public double getCost() {
        return 2.0;
    }
}

// Decorator base class
abstract class CoffeeDecorator implements Coffee {
    protected Coffee coffee;
    
    public CoffeeDecorator(Coffee coffee) {
        this.coffee = coffee;
    }
    
    @Override
    public String getDescription() {
        return coffee.getDescription();
    }
    
    @Override
    public double getCost() {
        return coffee.getCost();
    }
}

// Concrete decorators
class MilkDecorator extends CoffeeDecorator {
    public MilkDecorator(Coffee coffee) {
        super(coffee);
    }
    
    @Override
    public String getDescription() {
        return coffee.getDescription() + ", Milk";
    }
    
    @Override
    public double getCost() {
        return coffee.getCost() + 0.5;
    }
}

class SugarDecorator extends CoffeeDecorator {
    public SugarDecorator(Coffee coffee) {
        super(coffee);
    }
    
    @Override
    public String getDescription() {
        return coffee.getDescription() + ", Sugar";
    }
    
    @Override
    public double getCost() {
        return coffee.getCost() + 0.2;
    }
}

class WhippedCreamDecorator extends CoffeeDecorator {
    public WhippedCreamDecorator(Coffee coffee) {
        super(coffee);
    }
    
    @Override
    public String getDescription() {
        return coffee.getDescription() + ", Whipped Cream";
    }
    
    @Override
    public double getCost() {
        return coffee.getCost() + 0.7;
    }
}

// Usage - Stack decorators!
Coffee coffee1 = new SimpleCoffee();
System.out.println(coffee1.getDescription() + " $" + coffee1.getCost());

Coffee coffee2 = new MilkDecorator(new SimpleCoffee());
System.out.println(coffee2.getDescription() + " $" + coffee2.getCost());

Coffee coffee3 = new WhippedCreamDecorator(
                    new SugarDecorator(
                        new MilkDecorator(
                            new SimpleCoffee())));
System.out.println(coffee3.getDescription() + " $" + coffee3.getCost());
```

---

## 8. Facade Pattern

### Problem
You need to provide a **simple interface to a complex subsystem**.

### Real-World Analogy
Starting a car:
- Simple: Turn key
- Complex: Check fuel, ignite engine, engage transmission, etc.

The key is a facade!

### When to Use
- Simplify complex systems
- Provide a unified interface
- Decouple client from subsystem

### ✅ Facade Implementation

```java
// Complex subsystem classes
class CPU {
    public void freeze() {
        System.out.println("CPU: Freezing...");
    }
    
    public void jump(long position) {
        System.out.println("CPU: Jumping to " + position);
    }
    
    public void execute() {
        System.out.println("CPU: Executing...");
    }
}

class Memory {
    public void load(long position, byte[] data) {
        System.out.println("Memory: Loading data at " + position);
    }
}

class HardDrive {
    public byte[] read(long lba, int size) {
        System.out.println("HardDrive: Reading " + size + " bytes from " + lba);
        return new byte[size];
    }
}

// Facade - Simple interface
class ComputerFacade {
    private CPU cpu;
    private Memory memory;
    private HardDrive hardDrive;
    
    public ComputerFacade() {
        this.cpu = new CPU();
        this.memory = new Memory();
        this.hardDrive = new HardDrive();
    }
    
    public void start() {
        System.out.println("Starting computer...");
        cpu.freeze();
        memory.load(0, hardDrive.read(0, 1024));
        cpu.jump(0);
        cpu.execute();
        System.out.println("Computer started!\n");
    }
}

// Usage - Simple!
ComputerFacade computer = new ComputerFacade();
computer.start();  // One method call instead of many!
```

---

# Behavioral Patterns

Behavioral patterns deal with object interaction and responsibility.

---

## 9. Strategy Pattern

### Problem
You need to define a **family of algorithms** and make them interchangeable.

### Real-World Analogy
Navigation app:
- Walking route
- Driving route
- Public transport route

Same destination, different strategies!

### When to Use
- Multiple algorithms for a task
- Avoid if-else chains
- Runtime algorithm selection

### ✅ Strategy Implementation

```java
// Strategy interface
interface PaymentStrategy {
    void pay(double amount);
}

// Concrete strategies
class CreditCardStrategy implements PaymentStrategy {
    private String cardNumber;
    
    public CreditCardStrategy(String cardNumber) {
        this.cardNumber = cardNumber;
    }
    
    @Override
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " using Credit Card: " + cardNumber);
    }
}

class PayPalStrategy implements PaymentStrategy {
    private String email;
    
    public PayPalStrategy(String email) {
        this.email = email;
    }
    
    @Override
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " using PayPal: " + email);
    }
}

class CashStrategy implements PaymentStrategy {
    @Override
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " in cash");
    }
}

// Context
class ShoppingCart {
    private PaymentStrategy paymentStrategy;
    
    public void setPaymentStrategy(PaymentStrategy strategy) {
        this.paymentStrategy = strategy;
    }
    
    public void checkout(double amount) {
        if (paymentStrategy == null) {
            System.out.println("Please select a payment method");
            return;
        }
        paymentStrategy.pay(amount);
    }
}

// Usage
ShoppingCart cart = new ShoppingCart();

cart.setPaymentStrategy(new CreditCardStrategy("1234-5678-9012-3456"));
cart.checkout(100.0);

cart.setPaymentStrategy(new PayPalStrategy("user@example.com"));
cart.checkout(50.0);

cart.setPaymentStrategy(new CashStrategy());
cart.checkout(25.0);
```

### Real-World Example: Sorting Strategies

```java
interface SortStrategy {
    void sort(int[] array);
}

class BubbleSort implements SortStrategy {
    @Override
    public void sort(int[] array) {
        System.out.println("Sorting using Bubble Sort");
        // Implementation
    }
}

class QuickSort implements SortStrategy {
    @Override
    public void sort(int[] array) {
        System.out.println("Sorting using Quick Sort");
        // Implementation
    }
}

class MergeSort implements SortStrategy {
    @Override
    public void sort(int[] array) {
        System.out.println("Sorting using Merge Sort");
        // Implementation
    }
}

class Sorter {
    private SortStrategy strategy;
    
    public void setStrategy(SortStrategy strategy) {
        this.strategy = strategy;
    }
    
    public void sort(int[] array) {
        strategy.sort(array);
    }
}
```

---

## 10. Observer Pattern

### Problem
You need to notify multiple objects when another object changes state.

### Real-World Analogy
YouTube subscriptions:
- YouTuber uploads video (subject)
- All subscribers get notified (observers)

### When to Use
- One-to-many dependency
- Event handling systems
- Publish-subscribe systems

### ✅ Observer Implementation

```java
// Observer interface
interface Observer {
    void update(String message);
}

// Subject interface
interface Subject {
    void attach(Observer observer);
    void detach(Observer observer);
    void notifyObservers();
}

// Concrete subject
class YouTubeChannel implements Subject {
    private List<Observer> subscribers = new ArrayList<>();
    private String latestVideo;
    
    @Override
    public void attach(Observer observer) {
        subscribers.add(observer);
        System.out.println("New subscriber added!");
    }
    
    @Override
    public void detach(Observer observer) {
        subscribers.remove(observer);
        System.out.println("Subscriber removed!");
    }
    
    @Override
    public void notifyObservers() {
        for (Observer subscriber : subscribers) {
            subscriber.update(latestVideo);
        }
    }
    
    public void uploadVideo(String title) {
        this.latestVideo = title;
        System.out.println("\nNew video uploaded: " + title);
        notifyObservers();
    }
}

// Concrete observers
class Subscriber implements Observer {
    private String name;
    
    public Subscriber(String name) {
        this.name = name;
    }
    
    @Override
    public void update(String message) {
        System.out.println(name + " received notification: " + message);
    }
}

// Usage
YouTubeChannel channel = new YouTubeChannel();

Observer sub1 = new Subscriber("Alice");
Observer sub2 = new Subscriber("Bob");
Observer sub3 = new Subscriber("Charlie");

channel.attach(sub1);
channel.attach(sub2);
channel.attach(sub3);

channel.uploadVideo("Design Patterns Tutorial");
channel.uploadVideo("Java Best Practices");

channel.detach(sub2);
channel.uploadVideo("Advanced OOP Concepts");
```

---

## 11. Command Pattern

### Problem
You need to **encapsulate requests as objects**, allowing you to parameterize, queue, or log requests.

### Real-World Analogy
Restaurant:
- Waiter takes order (command)
- Order goes to kitchen (receiver)
- Chef executes order

### When to Use
- Undo/redo functionality
- Queue operations
- Log operations
- Macro recording

### ✅ Command Implementation

```java
// Command interface
interface Command {
    void execute();
    void undo();
}

// Receiver
class Light {
    public void turnOn() {
        System.out.println("Light is ON");
    }
    
    public void turnOff() {
        System.out.println("Light is OFF");
    }
}

// Concrete commands
class LightOnCommand implements Command {
    private Light light;
    
    public LightOnCommand(Light light) {
        this.light = light;
    }
    
    @Override
    public void execute() {
        light.turnOn();
    }
    
    @Override
    public void undo() {
        light.turnOff();
    }
}

class LightOffCommand implements Command {
    private Light light;
    
    public LightOffCommand(Light light) {
        this.light = light;
    }
    
    @Override
    public void execute() {
        light.turnOff();
    }
    
    @Override
    public void undo() {
        light.turnOn();
    }
}

// Invoker
class RemoteControl {
    private Command command;
    private Stack<Command> history = new Stack<>();
    
    public void setCommand(Command command) {
        this.command = command;
    }
    
    public void pressButton() {
        command.execute();
        history.push(command);
    }
    
    public void pressUndo() {
        if (!history.isEmpty()) {
            Command lastCommand = history.pop();
            lastCommand.undo();
        }
    }
}

// Usage
Light light = new Light();
Command lightOn = new LightOnCommand(light);
Command lightOff = new LightOffCommand(light);

RemoteControl remote = new RemoteControl();

remote.setCommand(lightOn);
remote.pressButton();  // Light is ON

remote.setCommand(lightOff);
remote.pressButton();  // Light is OFF

remote.pressUndo();    // Light is ON (undo)
remote.pressUndo();    // Light is OFF (undo)
```

---

## 12. Interpreter Pattern

### Problem
You need to **interpret and evaluate expressions** in a language.

### Real-World Analogy
Calculator:
- Input: "5 + 3 * 2"
- Interpret and evaluate: 11

### When to Use
- Simple grammar/language
- Expression evaluation
- Rule engines

### ✅ Interpreter Implementation

```java
// Abstract expression
interface Expression {
    int interpret();
}

// Terminal expressions
class NumberExpression implements Expression {
    private int number;
    
    public NumberExpression(int number) {
        this.number = number;
    }
    
    @Override
    public int interpret() {
        return number;
    }
}

// Non-terminal expressions
class AddExpression implements Expression {
    private Expression left;
    private Expression right;
    
    public AddExpression(Expression left, Expression right) {
        this.left = left;
        this.right = right;
    }
    
    @Override
    public int interpret() {
        return left.interpret() + right.interpret();
    }
}

class SubtractExpression implements Expression {
    private Expression left;
    private Expression right;
    
    public SubtractExpression(Expression left, Expression right) {
        this.left = left;
        this.right = right;
    }
    
    @Override
    public int interpret() {
        return left.interpret() - right.interpret();
    }
}

class MultiplyExpression implements Expression {
    private Expression left;
    private Expression right;
    
    public MultiplyExpression(Expression left, Expression right) {
        this.left = left;
        this.right = right;
    }
    
    @Override
    public int interpret() {
        return left.interpret() * right.interpret();
    }
}

// Usage: Build expression tree for "5 + 3 * 2"
Expression five = new NumberExpression(5);
Expression three = new NumberExpression(3);
Expression two = new NumberExpression(2);

Expression multiply = new MultiplyExpression(three, two);  // 3 * 2
Expression add = new AddExpression(five, multiply);        // 5 + (3 * 2)

System.out.println("Result: " + add.interpret());  // 11
```

---

# Pattern Recognition Guide

## When to Use Which Pattern?

### Problem: "Too many subclasses"
**Solution:** Bridge Pattern
```
Instead of: RedCircle, BlueCircle, RedSquare, BlueSquare...
Use: Shape + Color (separate hierarchies)
```

### Problem: "Too many if-else statements"
**Solution:** Strategy Pattern
```
Instead of: if (type == A) {...} else if (type == B) {...}
Use: Strategy interface with concrete strategies
```

### Problem: "Need exactly one instance"
**Solution:** Singleton Pattern
```
Database connection, Configuration, Logger
```

### Problem: "Complex object construction"
**Solution:** Builder Pattern
```
Many optional parameters, step-by-step construction
```

### Problem: "Interface mismatch"
**Solution:** Adapter Pattern
```
Legacy code, third-party libraries
```

### Problem: "Add functionality dynamically"
**Solution:** Decorator Pattern
```
Coffee + Milk + Sugar, Text + Bold + Italic
```

### Problem: "Simplify complex system"
**Solution:** Facade Pattern
```
Hide complexity behind simple interface
```

### Problem: "Notify multiple objects"
**Solution:** Observer Pattern
```
Event systems, subscriptions
```

### Problem: "Undo/redo functionality"
**Solution:** Command Pattern
```
Text editor, game actions
```

## Pattern Comparison

| Pattern | Purpose | Example |
|---------|---------|---------|
| **Factory** | Create objects | ShapeFactory.create("circle") |
| **Builder** | Complex construction | new Pizza.Builder().cheese().build() |
| **Adapter** | Interface compatibility | USBAdapter for old devices |
| **Bridge** | Separate abstraction/implementation | Shape + Color |
| **Decorator** | Add functionality | Coffee + Milk + Sugar |
| **Strategy** | Interchangeable algorithms | Different payment methods |
| **Observer** | Notify dependents | YouTube subscriptions |
| **Command** | Encapsulate requests | Remote control buttons |

## Summary

Design patterns are tools, not rules. Use them when they solve your problem, not because they're "cool"!

**Key Takeaways:**
- Patterns solve recurring problems
- They provide a common vocabulary
- Don't over-engineer simple solutions
- Understand the problem before applying a pattern
- Patterns often work together

**Practice:** Try to identify patterns in frameworks you use (Spring, Android, etc.)!