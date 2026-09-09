# IoC and Dependency Injection in Java

## 1. Basic Problem

Suppose we have two classes:

```java
class Engine {

    void start() {
        System.out.println("Engine started");
    }
}

class Car {

    void drive() {

        Engine engine = new Engine();

        engine.start();

        System.out.println("Car is driving");
    }
}
```

Here, `Car` creates the `Engine` object itself:

```java
Engine engine = new Engine();
```

The relationship is:

```text
Car
 |
 | creates
 ↓
Engine
```

This means `Car` is responsible for creating and managing its dependency.

---

# 2. What is a Dependency?

If one class needs another class to perform its work, the second class is called a **dependency**.

Example:

```java
class Car {

    private Engine engine;
}
```

Here:

```text
Car → depends on → Engine
```

Therefore:

> `Engine` is a dependency of `Car`.

---

# 3. Dependency Injection (DI)

Instead of `Car` creating the `Engine`, we provide the `Engine` from outside.

```java
class Car {

    private Engine engine;

    Car(Engine engine) {
        this.engine = engine;
    }

    void drive() {
        engine.start();
    }
}
```

Now the `Car` does **not** create the `Engine`.

The outside code creates it:

```java
Engine engine = new Engine();

Car car = new Car(engine);
```

The dependency is being **injected** into `Car`.

```text
Outside code
     |
     | creates Engine
     ↓
   Engine
     |
     | injects
     ↓
    Car
```

This is called **Dependency Injection**.

---

# 4. Types of Dependency Injection

There are three commonly discussed types:

1. Constructor Injection
2. Setter Injection
3. Field Injection

Method/parameter injection is also possible in general Java design.

---

# 5. Constructor Injection

The dependency is provided through the constructor.

```java
class Car {

    private Engine engine;

    Car(Engine engine) {
        this.engine = engine;
    }

    void drive() {
        engine.start();
    }
}
```

Usage:

```java
Engine engine = new Engine();

Car car = new Car(engine);

car.drive();
```

The important part is:

```java
Car car = new Car(engine);
```

The `Engine` is provided when the `Car` object is created.

Therefore:

> **Dependency is injected through the constructor → Constructor Injection**

## Why use Constructor Injection?

It is useful when the dependency is **required**.

For example:

```java
class Car {

    private final Engine engine;

    Car(Engine engine) {
        this.engine = engine;
    }
}
```

A `Car` cannot be created without an `Engine`.

```java
Car car = new Car();  // Compilation error
```

You must provide the dependency:

```java
Car car = new Car(engine);
```

---

# 6. Setter Injection

Instead of passing the dependency through the constructor, we provide it through a setter method.

```java
class Car {

    private Engine engine;

    public void setEngine(Engine engine) {
        this.engine = engine;
    }

    void drive() {
        engine.start();
    }
}
```

Usage:

```java
Car car = new Car();

Engine engine = new Engine();

car.setEngine(engine);

car.drive();
```

Here:

```java
car.setEngine(engine);
```

injects the dependency.

Therefore:

> **Dependency is injected through a setter → Setter Injection**

### Flow

```text
Create Car
    ↓
Car car = new Car();

Create Engine
    ↓
Engine engine = new Engine();

Inject Engine
    ↓
car.setEngine(engine);

Car now has Engine
```

Setter injection can be useful when a dependency is **optional** or can be changed after object creation.

---

# 7. Constructor Injection vs Setter Injection

## Constructor Injection

```java
Car car = new Car(engine);
```

Dependency is provided during object creation.

## Setter Injection

```java
Car car = new Car();

car.setEngine(engine);
```

Dependency is provided after object creation.

### Comparison

| Constructor Injection                                | Setter Injection                   |
| ---------------------------------------------------- | ---------------------------------- |
| Dependency provided through constructor              | Dependency provided through setter |
| Dependency available immediately                     | Dependency provided later          |
| Good for required dependencies                       | Good for optional dependencies     |
| Object cannot be created without required dependency | Object can be created first        |
| Generally preferred in Spring                        | Useful in certain cases            |

---

# 8. Field Injection

The dependency is directly placed into a field.

In Spring:

```java
class Car {

    @Autowired
    private Engine engine;

    void drive() {
        engine.start();
    }
}
```

Spring injects the `Engine` directly into the field.

You don't explicitly write:

```java
Engine engine = new Engine();
```

and you don't write:

```java
car.setEngine(engine);
```

Spring handles the injection.

### Why field injection is generally not preferred

The dependency is hidden inside the class.

Compare:

```java
class Car {

    @Autowired
    private Engine engine;
}
```

with:

```java
class Car {

    private final Engine engine;

    Car(Engine engine) {
        this.engine = engine;
    }
}
```

The second version clearly tells anyone using the class:

> `Car` requires an `Engine`.

---

# 9. Method/Parameter Injection

A dependency can also be passed directly to a method.

```java
class Car {

    void drive(Engine engine) {
        engine.start();
    }
}
```

Usage:

```java
Car car = new Car();

Engine engine = new Engine();

car.drive(engine);
```

Here `Engine` is available only to the `drive()` method.

This can be useful when the dependency is required only for a specific operation.

---

# 10. What is IoC?

IoC stands for:

> **Inversion of Control**

Normally, your class controls the creation of its dependencies:

```java
class Car {

    private Engine engine = new Engine();
}
```

Here:

```text
Car
 |
 | creates
 ↓
Engine
```

`Car` controls the creation of `Engine`.

---

With IoC, the responsibility is moved outside the class.

```java
class Car {

    private Engine engine;

    Car(Engine engine) {
        this.engine = engine;
    }
}
```

Now someone else provides the `Engine`.

```text
Outside
   |
   | creates Engine
   ↓
 Engine
   |
   | provides
   ↓
  Car
```

The control over dependency creation has been **inverted**.

---

# 11. IoC vs Dependency Injection

These two terms are related but not identical.

### IoC

IoC is a **design principle**.

> Control of object creation and dependency management is transferred from the class/application to another component or framework.

### Dependency Injection

DI is a **technique for implementing IoC**.

> Dependencies are provided to a class from outside rather than being created by the class itself.

Relationship:

```text
IoC
 |
 └── Dependency Injection
       |
       ├── Constructor Injection
       ├── Setter Injection
       └── Field Injection
```

---

# 12. Interface and Dependency Injection

In real applications, we often use an interface instead of depending directly on a concrete class.

Instead of:

```java
class Car {

    private PetrolEngine engine;

    Car() {
        engine = new PetrolEngine();
    }
}
```

Create an interface:

```java
interface Engine {

    void start();
}
```

Implement it:

```java
class PetrolEngine implements Engine {

    public void start() {
        System.out.println("Petrol engine started");
    }
}
```

Another implementation:

```java
class DieselEngine implements Engine {

    public void start() {
        System.out.println("Diesel engine started");
    }
}
```

Now `Car` depends on the interface:

```java
class Car {

    private Engine engine;

    Car(Engine engine) {
        this.engine = engine;
    }

    void drive() {
        engine.start();
    }
}
```

We can provide either implementation:

```java
Engine engine = new PetrolEngine();

Car car = new Car(engine);
```

or:

```java
Engine engine = new DieselEngine();

Car car = new Car(engine);
```

`Car` doesn't need to change.

---

# 13. Loose Coupling

This is one of the major benefits of DI.

### Tightly coupled

```java
class Car {

    private PetrolEngine engine = new PetrolEngine();
}
```

`Car` directly depends on `PetrolEngine`.

```text
Car → PetrolEngine
```

If we want `DieselEngine`, we have to modify `Car`.

---

### Loosely coupled

```java
class Car {

    private Engine engine;

    Car(Engine engine) {
        this.engine = engine;
    }
}
```

Now:

```text
             Engine
             /    \
            /      \
 PetrolEngine    DieselEngine
            \      /
             \    /
              Car
```

`Car` only knows about the `Engine` interface.

This makes the application more flexible and easier to test.

---

# 14. How Spring Uses IoC

Spring provides an **IoC Container**.

For example:

```java
@Component
class Engine {

    void start() {
        System.out.println("Engine started");
    }
}
```

And:

```java
@Component
class Car {

    private final Engine engine;

    @Autowired
    Car(Engine engine) {
        this.engine = engine;
    }

    void drive() {
        engine.start();
    }
}
```

Spring sees that `Engine` is a component and creates an object for it.

Then Spring sees that `Car` requires an `Engine`.

Conceptually, Spring does something similar to:

```java
Engine engine = new Engine();

Car car = new Car(engine);
```

You don't manually write those `new` statements.

Spring manages them.

---

# 15. Spring IoC Container

The Spring container manages objects called **Beans**.

```text
             Spring IoC Container
                     |
          ┌──────────┴──────────┐
          ↓                     ↓
      Engine Bean           Car Bean
          |                     |
          └────── injected ─────┘
```

Spring can manage:

* Object creation
* Dependency Injection
* Bean lifecycle
* Configuration
* Bean scope

---

# 16. `@Component`

When you write:

```java
@Component
class Engine {
}
```

you're telling Spring:

> "Spring, manage an object of this class as a bean."

Similarly:

```java
@Component
class Car {
}
```

Spring manages the `Car` object.

---

# 17. `@Autowired`

`@Autowired` tells Spring where/how to resolve a dependency.

Example:

```java
@Component
class Car {

    private final Engine engine;

    @Autowired
    Car(Engine engine) {
        this.engine = engine;
    }
}
```

Spring finds an appropriate `Engine` bean and passes it to the constructor.

In modern Spring, when a class has **only one constructor**, `@Autowired` can usually be omitted:

```java
@Component
class Car {

    private final Engine engine;

    Car(Engine engine) {
        this.engine = engine;
    }
}
```

Spring automatically uses that constructor.

---

# 18. `new` vs Dependency Injection

### Traditional

```java
class Car {

    Engine engine = new Engine();
}
```

The class creates the dependency.

```text
Car → new Engine()
```

### Dependency Injection

```java
class Car {

    private Engine engine;

    Car(Engine engine) {
        this.engine = engine;
    }
}
```

Someone else creates and provides the dependency.

```text
Outside/Spring
      |
      ↓
   Engine
      |
      ↓
     Car
```

---

# 19. The Complete Relationship

```text
                     IoC
                      |
             Design Principle
                      |
                      ↓
             Dependency Injection
                      |
          ┌───────────┼───────────┐
          ↓           ↓           ↓
   Constructor     Setter       Field
    Injection     Injection    Injection
          |
          ↓
   Most commonly preferred
    in Spring applications
```

And with Spring:

```text
                  Spring
               IoC Container
                     |
          ┌──────────┴──────────┐
          ↓                     ↓
       Engine                  Car
        Bean                    Bean
          |                     |
          |                     |
          └──── dependency ─────┘
                    ↓
             Constructor
              Injection
```

---

# 20. Interview Summary

### What is IoC?

> IoC is a design principle where control of object creation and dependency management is transferred from the application code to a container or framework.

### What is Dependency Injection?

> Dependency Injection is a technique where an object's dependencies are provided from outside instead of the object creating them itself.

### What is Constructor Injection?

> Providing a dependency through the class constructor.

```java
Car(Engine engine) {
    this.engine = engine;
}
```

### What is Setter Injection?

> Providing a dependency through a setter method.

```java
void setEngine(Engine engine) {
    this.engine = engine;
}
```

### What is Field Injection?

> Injecting a dependency directly into a field, commonly using `@Autowired`.

```java
@Autowired
private Engine engine;
```

### Which injection is generally preferred in Spring?

> **Constructor Injection**, especially for required dependencies, because dependencies are explicit, the object can be immutable, and testing is easier.

### Simple way to remember

```text
Without DI:

Car creates Engine
       ↓
new Engine()

With DI:

Someone else creates Engine
       ↓
Engine → Car

With Spring:

Spring creates Engine
       ↓
Spring injects Engine into Car
```

**One-line concept:**

> **IoC = Who controls object creation?**
> **DI = How is the dependency given to the object?**
