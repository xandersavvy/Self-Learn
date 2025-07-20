# Top Core Java Interview & Certification Exam Questions

This curated list of **100** essential Core Java questions covers both interview preparation and Oracle Java SE programming certification (OCA 1Z0-808) topics. It omits outdated applet/servlet content and meta-level questions. Master these to confidently interview and pass the Java SE 8 Programmer I exam.

## 1. Java Basics & Language Fundamentals  
1. Explain the Java platform architecture (JVM, JRE, JDK).  
2. Describe the Java compilation and execution process.  
3. Why is Java platform independent?  
4. Compare `==` vs. `equals()` for objects.  
5. What are primitive types and their default values?  
6. How do Java literals, constants, and variables differ?  
7. What are scope and lifetime of local, instance, and static variables?  
8. Explain boxing/unboxing and wrapper classes.  
9. What is the purpose of the `final` keyword for classes, methods, and variables?  
10. Describe the use of `static` for methods, fields, and nested classes.  

## 2. Object-Oriented Programming  
11. Define class and object in Java.  
12. Explain encapsulation, inheritance, polymorphism, and abstraction.  
13. What is method overloading vs. overriding?  
14. How does Java implement multiple inheritance?  
15. When use an abstract class vs. interface?  
16. What are default and private methods in interfaces (Java 8+)?  
17. Describe the `super` keyword in constructors and methods.  
18. How are polymorphic calls resolved at runtime?  
19. Explain the role of marker interfaces (`Serializable`, `Cloneable`).  
20. What are nested, inner, static nested, and local classes?  

## 3. Flow Control & Exception Handling  
21. Compare `if–else`, `switch`, and ternary operator usage.  
22. What are the rules for `switch` on `enum` and `String`?  
23. Explain `for`, enhanced `for`, `while`, and `do–while` loops.  
24. What are labeled statements and when to use them?  
25. Distinguish between checked and unchecked exceptions.  
26. How do `try–catch–finally` blocks execute under normal and exceptional flows?  
27. What exceptions propagate out of `main()` if uncaught?  
28. When does the `finally` block not execute?  
29. How to create and throw custom exceptions?  
30. Explain exception chaining with `initCause()`.  

## 4. Java Memory Management & Garbage Collection  
31. What is the Java memory model: heap vs. stack?  
32. How does garbage collection work?  
33. Describe generational GC and common collectors (G1, CMS).  
34. What are strong, soft, weak, and phantom references?  
35. Explain finalization and `ReferenceQueue`.  
36. When use `System.gc()` or `Runtime.getRuntime().gc()`?  
37. What are memory leaks in Java and how to detect them?  
38. How to tune heap sizes (`-Xms`, `-Xmx`)?  
39. Explain escape analysis and stack allocation.  
40. How do Java 8 and later handle compressed OOPs?  

## 5. Java APIs & Core Libraries  
41. Compare `String`, `StringBuilder`, and `StringBuffer`.  
42. How does `String` pooling work?  
43. Explain `java.time` API vs. legacy `java.util.Date`.  
44. What are the primitive and boxed numeric wrapper classes?  
45. How to convert between collections and arrays?  
46. Describe `Optional` use and pitfalls.  
47. Explain `EnumSet` and `EnumMap`.  
48. What is `java.util.Properties` for configuration?  
49. How do `ResourceBundle` and localization work?  
50. Explain class loading and the delegation model.  

## 6. Concurrency & Multithreading  
51. Difference between process and thread.  
52. How to create threads via `Thread` vs. `Runnable` vs. `Callable`.  
53. What are `synchronized` methods and blocks?  
54. Explain intrinsic locks vs. explicit `Lock` interfaces.  
55. What is the Java Memory Model (JMM)?  
56. Explain `volatile` variables and visibility.  
57. Compare `wait()`/`notify()` with `Lock`/`Condition`.  
58. Describe `CountDownLatch`, `CyclicBarrier`, `Semaphore`, and `Exchanger`.  
59. What are thread pools via `ExecutorService`?  
60. How to avoid deadlocks, livelocks, and starvation.  

## 7. Generics & Collections Framework  
61. What are Java generics and type erasure?  
62. Why use bounded type parameters and wildcards (`? extends`, `? super`)?  
63. Compare `List`, `Set`, `Map`, and their key implementations.  
64. What are fail-fast vs. fail-safe iterators?  
65. Explain capacity and load factor for `HashMap` and `HashSet`.  
66. How `TreeMap` and `TreeSet` support sorting via `Comparable`/`Comparator`.  
67. What are `ConcurrentHashMap` and `CopyOnWriteArrayList` for concurrency?  
68. How to use `Collections.unmodifiable*`, `synchronized*`, and `checked*` wrappers.  
69. Describe `Deque` and implementations (`ArrayDeque`, `LinkedList`).  
70. How to use `NavigableMap` and `NavigableSet` methods.  

## 8. I/O & NIO.2  
71. Differences between `java.io` and `java.nio`.  
72. How to read/write files with `FileInputStream`/`FileOutputStream`.  
73. Explain `BufferedReader`, `BufferedWriter`, and why buffer.  
74. How to traverse file systems with NIO.2 (`Path`, `Files`, `FileVisitor`).  
75. What are channels and buffers in NIO?  
76. Describe asynchronous I/O with `AsynchronousFileChannel`.  
77. How to use `WatchService` for directory monitoring.  
78. Explain `ObjectOutputStream`/`ObjectInputStream` for serialization.  
79. What is externalizable vs. serializable?  
80. How to customize serialization with `writeObject()`, `readObject()`.  

## 9. Lambda, Streams & Functional Programming  
81. What are lambda expressions and functional interfaces?  
82. How to use built-in functional interfaces (`Predicate`, `Function`, `Consumer`).  
83. Explain method references (`Class::staticMethod`, `instance::method`).  
84. Compare imperative vs. declarative style with Streams.  
85. How to create, transform, filter, and collect streams.  
86. What are parallel streams and thread-safety concerns?  
87. Explain primitive streams (`IntStream`, `LongStream`, `DoubleStream`).  
88. How to use `Optional` to avoid null checks in streams.  
89. Describe pipeline vs. terminal operations.  
90. How to handle checked exceptions in lambdas/streams.  

## 10. Java SE 8 Certification Focus Areas  
91. Define class structure, `main()` signature, command-line invocation.  
92. Declare, initialize, and cast primitive and reference types.  
93. Explain wrapper classes and auto-boxing/unboxing.  
94. Write code using arrays and control flow statements.  
95. Create, apply, and override methods; use `static`, `final`, and access modifiers.  
96. Define and implement inheritance hierarchies; use `super` and polymorphism.  
97. Work with interfaces: `default` & `static` methods.  
98. Handle exceptions with `try`/`catch`/`finally`; understand the exception hierarchy.  
99. Use Java Collections: `List`, `Set`, `Map`; iterate with `Iterator` & `for`-each.  
100. Apply Java 8 features: lambda expressions, streams, and `java.time` package.  

Mastering these topics and practicing related code snippets will prepare you for Core Java interviews and ensure success on the Oracle Java SE 8 Programmer I (1Z0-808) certification exam.

[1] https://www.dbexam.com/oracle/oracle-1z0-808-certification-exam-syllabus
[2] https://www.ksclnmu.ac.in/web/admin/pdf/202110091155042021100911542620211009115226pdf20211008112956pdf20211008034253pdf20211007093406240-core-java-interview-questions-and-answers.pdf
[3] https://www.examtopics.com/exams/oracle/1z0-808/view/
[4] https://www.youtube.com/watch?v=EsBUD8nuVeE
[5] https://www.oracle.com/in/education/certification/
[6] https://www.digitalocean.com/community/tutorials/core-java-interview-questions-and-answers
[7] https://www.examtopics.com/exams/oracle/1z0-808/
[8] https://utho.com/blog/java-programming-interview-questions/
[9] https://www.examtopics.com/exams/oracle/1z0-819/
[10] https://www.guvi.in/blog/40-java-interview-questions-for-freshers/
[11] https://www.examtopics.com/exams/oracle/1z0-900/
[12] https://www.interviewbit.com/java-interview-questions/
[13] https://www.examtopics.com/exams/oracle/
[14] https://www.geeksforgeeks.org/java/java-interview-questions/
[15] https://education.oracle.com/oracle-certification-path/pFamily_48
[16] https://codefinity.com/blog/The-80-Top-Java-Interview-Questions-and-Answers
[17] https://education.oracle.com/java-se-21-developer-professional/pexam_1Z0-830
[18] https://www.datacamp.com/blog/java-interview-questions
[19] https://education.oracle.com/java-se-8-programmer-i/pexam_1Z0-808
[20] https://javatechonline.com/core-java-interview-questions-and-answers/