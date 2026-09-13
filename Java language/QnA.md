
## $\color{red}{\text{1. What is Java?}}$  
**Interview answer:**

Java is a **high-level, object-oriented, class-based programming language** designed to be platform independent.

Java source code is compiled into **bytecode**, which runs on the JVM. Because JVM implementations exist for different operating systems, the same bytecode can run across platforms.

```text
Java Source Code
       ↓
    javac
       ↓
   Bytecode (.class)
       ↓
      JVM
       ↓
Operating System
```

### Key point

> **Write Once, Run Anywhere (WORA)** is possible because Java runs through the JVM.

---

## 2. Why is Java platform independent?


Java is platform independent because Java code is compiled into **platform-independent bytecode**, rather than directly into OS-specific machine code.

```java
Hello.java
   ↓
javac
   ↓
Hello.class
   ↓
JVM
   ↓
Windows / Linux / macOS
```

The JVM is platform dependent, but the **bytecode is platform independent**.

### Important interview statement

> Java itself is platform independent, while JVM implementations are platform dependent.

---

# 3. What is JVM, JRE and JDK?

This is a **very common interview question**.

### JVM — Java Virtual Machine

JVM executes Java bytecode.

Responsibilities include:

* Loading classes
* Bytecode verification
* Executing bytecode
* Memory management
* Garbage collection
* JIT compilation

### JRE — Java Runtime Environment

Conceptually:

```text
JRE = JVM + Java Runtime Libraries
```

It provides the environment required to **run** Java applications.

### JDK — Java Development Kit

Conceptually:

```text
JDK = JRE + Development Tools
```

It contains tools such as:

```text
javac
java
javadoc
jar
jdb
```

### Simple diagram

```text
JDK
├── Development Tools
└── JRE
    ├── Java Libraries
    └── JVM
```

### Modern Java note

Since Java 9 and later, the old separate-JRE distribution model changed significantly, so don't make the simplistic claim that every modern JDK installation contains a separately installed JRE.

---

# 4. What happens when you compile and run a Java program?

Suppose:

```java
public class Test {
    public static void main(String[] args) {
        System.out.println("Hello");
    }
}
```

### Compilation

```bash
javac Test.java
```

The Java compiler converts:

```text
Test.java
   ↓
Test.class
```

The `.class` file contains **bytecode**.

### Execution

```bash
java Test
```

Then approximately:

```text
Class Loader
     ↓
Bytecode Verification
     ↓
JVM Runtime
     ↓
Interpreter / JIT Compiler
     ↓
Machine Code
     ↓
CPU
```

### 7-YOE answer

You can say:

> The compiler converts Java source code into bytecode. At runtime, the JVM loads and verifies the bytecode and executes it using interpretation and JIT compilation. The JVM also manages runtime memory and garbage collection.

---

# 5. Why is Java not 100% object-oriented?

Because Java supports **primitive data types**.

Examples:

```java
int
char
boolean
double
float
long
short
byte
```

These aren't objects.

For example:

```java
int age = 25;
```

`age` is a primitive value, not an object.

Java provides wrapper classes:

```java
Integer
Character
Boolean
Double
```

So Java is strongly object-oriented, but not purely object-oriented.

---

# 6. What are primitive and non-primitive data types?

### Primitive

Java has 8 primitive types:

```text
byte
short
int
long
float
double
char
boolean
```

Example:

```java
int age = 30;
double salary = 50000.50;
boolean active = true;
```

### Non-primitive/reference types

Examples:

```java
String
Array
Class
Interface
Object
```

Example:

```java
String name = "Jayesh";
Employee employee = new Employee();
```

### Main difference

Primitive types represent values directly.

Reference variables refer to objects.

---

# 7. Difference between `int` and `Integer`?

```java
int a = 10;
Integer b = 10;
```

### `int`

* Primitive
* Cannot be `null`
* Generally more memory-efficient
* Doesn't have methods

### `Integer`

* Wrapper class
* Can be `null`
* Has methods
* Can be used where an object is required

Example:

```java
Integer x = null;
```

This is valid.

But:

```java
int x = null;
```

is invalid.

### Why do we need Integer?

Collections use objects:

```java
List<Integer> numbers = new ArrayList<>();
```

You can't write:

```java
List<int> numbers;
```

---

# 8. What is autoboxing and unboxing?

### Autoboxing

Primitive → Wrapper automatically.

```java
int x = 10;

Integer y = x;
```

Java automatically converts:

```text
int → Integer
```

### Unboxing

Wrapper → Primitive.

```java
Integer x = 10;

int y = x;
```

```text
Integer → int
```

### Important trap

```java
Integer x = null;

int y = x;
```

This causes:

```text
NullPointerException
```

because Java attempts to unbox `null`.

---

# 9. What is a variable?

A variable is a named storage location used to hold a value.

Example:

```java
int age = 30;
```

Here:

```text
int  → data type
age  → variable
30   → value
```

Java has three important categories:

```text
Local variable
Instance variable
Static/Class variable
```

---

# 10. Local vs instance vs static variable

### Local variable

Declared inside a method/block.

```java
void test() {
    int x = 10;
}
```

It doesn't receive a default value automatically.

You must initialize it before use.

---

### Instance variable

Declared inside a class but outside methods.

```java
class Employee {
    String name;
    int age;
}
```

Each object gets its own copy.

```java
Employee e1 = new Employee();
Employee e2 = new Employee();
```

`e1.name` and `e2.name` are independent.

---

### Static variable

Belongs to the class rather than individual objects.

```java
class Employee {
    static String company = "ABC";
}
```

All objects share the same static variable.

---

# 11. What are the default values of instance variables?

Java automatically assigns default values to instance variables.

| Type      | Default    |
| --------- | ---------- |
| `byte`    | `0`        |
| `short`   | `0`        |
| `int`     | `0`        |
| `long`    | `0L`       |
| `float`   | `0.0f`     |
| `double`  | `0.0d`     |
| `char`    | `'\u0000'` |
| `boolean` | `false`    |
| Reference | `null`     |

Example:

```java
class Test {
    int x;
    boolean flag;
    String name;
}
```

Conceptually:

```text
x     = 0
flag  = false
name  = null
```

### Important

Local variables **don't get default values**.

```java
void test() {
    int x;
    System.out.println(x); // compilation error
}
```

---

# 12. What is type casting?

Converting one data type into another.

There are two major forms.

### Widening

Smaller range → larger range.

```java
int x = 10;
long y = x;
```

Usually happens automatically.

```text
int → long → float → double
```

### Narrowing

Larger type → smaller type.

```java
double x = 10.5;
int y = (int) x;
```

Here:

```text
10.5 → 10
```

Data may be lost.

---

# 13. Implicit vs explicit casting

### Implicit

Java automatically converts the type.

```java
int x = 100;
long y = x;
```

No explicit cast needed.

### Explicit

You tell Java to convert it.

```java
double x = 10.5;
int y = (int) x;
```

The `(int)` is explicit casting.

### Interview point

> Widening conversion is generally safe, while narrowing conversion may result in data loss.

---

# 14. Difference between `==` and `.equals()`?

This is **extremely important**.

### `==`

For primitives:

> Compares values.

```java
int a = 10;
int b = 10;

System.out.println(a == b); // true
```

For objects:

> Compares references/identity.

```java
String a = new String("hello");
String b = new String("hello");

a == b;        // false
a.equals(b);   // true
```

Why?

Because:

```text
a ──→ String object "hello"
b ──→ String object "hello"
```

They are different objects.

`.equals()` checks logical equality according to the class's implementation.

---

# 15. What is the `main()` method?

The traditional Java application entry point is:

```java
public static void main(String[] args)
```

### `public`

JVM needs to access the method.

### `static`

JVM can invoke it without creating an object.

### `void`

It doesn't return a value.

### `main`

The conventional entry-point method name.

### `String[] args`

Receives command-line arguments.

Example:

```bash
java Test hello world
```

Then:

```java
args[0] = "hello"
args[1] = "world"
```

---

# 16. Can we overload the `main()` method?

**Yes.**

Example:

```java
public static void main(String[] args) {
    System.out.println("Entry point");
}

public static void main(int x) {
    System.out.println(x);
}
```

This is valid **method overloading**.

But the JVM looks for the recognized entry-point signature.

So:

```java
main(String[] args)
```

is the important one for launching a normal Java application.

---

# 17. Can we make `main()` non-static?

For the traditional Java application entry point:

**No.**

If you write:

```java
public void main(String[] args)
```

the JVM won't treat it as the normal static entry point.

Why is `static` needed?

Because JVM needs to invoke `main()` **without first creating an instance of the class**.

---

# 18. What happens if `main()` is not present?

The class can still compile:

```bash
javac Test.java
```

But if you try:

```bash
java Test
```

the JVM cannot find the required application entry point and reports that the main method is missing.

---

# 19. What are command-line arguments?

They are values passed to a Java application when starting it.

Example:

```bash
java Test Jayesh 30
```

Code:

```java
public static void main(String[] args) {
    System.out.println(args[0]);
    System.out.println(args[1]);
}
```

Output:

```text
Jayesh
30
```

All command-line arguments arrive as **Strings**.

---

# 20. Is Java pass-by-value or pass-by-reference?

🔥 **Very common interview trap.**

**Java is always pass-by-value.**

Even when passing objects, Java passes a **copy of the reference value**.

Example:

```java
class Employee {
    String name;
}

void change(Employee e) {
    e.name = "John";
}
```

Calling:

```java
Employee emp = new Employee();
emp.name = "Jayesh";

change(emp);
```

changes the object's name because the copied reference still points to the same object.

But:

```java
void change(Employee e) {
    e = new Employee();
    e.name = "John";
}
```

doesn't change the caller's `emp` reference.

### Best interview explanation

```text
Caller
 emp
  │
  └────────→ Object A

             ↓ pass value

Method
 e
  │
  └────────→ Object A
```

`emp` and `e` contain references to the same object, but **the reference itself was copied**.

Therefore:

> **Java is strictly pass-by-value; for objects, the value being passed is the reference value.**

---

# ⭐ Part 1 — What you should be able to answer confidently

For a **7 YOE Java interview**, don't merely memorize these.

You should be comfortable explaining:

```text
Java
 ↓
JDK / JRE / JVM
 ↓
Compilation
 ↓
Bytecode
 ↓
Class loading
 ↓
JVM execution
 ↓
Heap / Stack
 ↓
Objects
 ↓
References
 ↓
Pass-by-value
```

And particularly remember these **interview traps**:

1. **Java is platform independent; JVM is platform dependent.**
2. **Java is not purely object-oriented because of primitives.**
3. **`==` compares references for objects; `.equals()` compares logical equality.**
4. **Java is always pass-by-value.**
5. **Static belongs to the class, instance variables belong to objects.**
6. **Local variables don't receive default values.**
7. **`Integer` can be `null`; `int` cannot.**
8. **Autoboxing/unboxing can cause `NullPointerException`.**

**Next: Part 2 — OOP (Q21–47)** is where the interview starts getting substantially more interesting, including **overloading vs overriding, static/private method behavior, abstract class vs interface, multiple inheritance, composition, `this`/`super`, and common 7-YOE follow-ups.**
