# Java - Elements of Object-Oriented Programming (Part I)
## 1. Concepts of Encapsulation, Inheritance, and Polymorphism

### Exercises

---

1. **Explain the concept of encapsulation and the way it is implemented in Java.**

**Encapsulation** is often described as one or more of the following:

```
    - Bundling together publicly accessible methods and privately accessible data.
    - Data hiding (a controversial definition, see the linked discussion below).
    - Controlled access to an object's properties.
``` 

In Java, encapsulation is implemented through classes. A class groups data (fields) and the methods that operate on that data, while access modifiers (e.g., private, protected, public) control how the data can be accessed from outside the class.

---

2. Explain the following concepts:

   - **Mutator method (setter)**

    - **Accessor method (getter)**
```
    - A **mutator** method (including setters) changes the state of an object.
    - An **accessor** method (including getters) retrieves the state of an object.
```

---

3. Explain two different meanings/roles of:

   - ```this```

   - ```super```
```
    - "this" has three roles: 
        1. refers to the current object (to disambiguate fields from parameters, e.g. this.pfd1 = pfd1;); 
        2. calls another constructor of the same class (this(1.0);); 
        3.can be passed as an argument to another method.
    - "super" has two roles: 
        1. refers to the superclass, used to access overridden methods or hidden fields (super.b2 = 4;); 
        2. calls a superclass constructor (super(x);), and it must be the first statement in the constructor body.```

            NB : super cannot access private members of the parent
```

---

4. Explain the concept of inheritance and the way it is implemented in Java.

**Inheritance** can be understood in two distinct senses:

```
    - "Implementation/code inheritance" (analogous to private inheritance in C++) is a form of code reuse. In Java, it is implemented through the extends and super keywords.

    - "Subtyping" (interface inheritance, analogous to public inheritance in C++) refers to compatibility of interfaces and establishes an IS-A relationship.
```

---

5. Explain the concept of polymorphism, name its three main kinds/forms, and explain the way they are implemented in Java.

There are several kinds of polymorphism:

- **Ad-hoc polymorphism** (overloading of operators, functions, or as in Java methods).

  - Implemented through the same method name with a different number and/or type of parameters.

``` java
public class Circle extends Shape {
    private double r;

    @Override
    public double area() {
        return Math.PI * r * r;
    }

    @Override
    public double perimeter() {
        return 2 * Math.PI * r;
    }

    /**
     * Overloaded constructor (ad-hoc polymorphis, compile-time)
     */
    public Circle(double r, boolean filed) {
        super(filed); // -> Shape(boolean filled)
        this.r = r;
    }

    /**
     * Overloaded constructor (ad-hoc polymorphism, compile-time)
     */
    public Circle(double r) {
        this(r, true); // -> Circle(double r, boolean filed)
    }
}
```

- **Subtype/inclusion polymorphism** (as in the example below).
    
  - Implemented through the ability of a variable to refer to instances of a whole family of types derived from a common base type. Method calls are resolved at runtime based on the actual object type (dynamic dispatch).

``` java
    /**
     * Subtype polymorphism - ability of a reference variable to take different forms
     */
    private static double totalArea(List<Shape> shapes) {
        double totArea = 0;

        // s is polymorphic - it can refer to instances of the whole family of types derived from 'Shape'
        for (Shape s : shapes) {
            totArea += s.area();
        }

        return totArea;
    }
```

- **Parametric polymorphism** (Java Generics).

  - Implemented through generic types and methods, allowing code to be written independently of specific types while preserving compile-time type safety.

```java
    // Generic method — T is a type parameter
    static <T> T firstOrNull(List<T> list) {
        return list.isEmpty() ? null : list.get(0);
    }

    // Generic class
    class Box<T> {
        private final T value;
        Box(T value) { this.value = value; }
        T get() { return value; }
    }
```

---

6. Explain the relationship between inheritance and subtype/inclusion polymorphism.
```
   - Inheritance is about "reusing code"; subtyping is about "being substitutable".

   - Subtype/inclusion polymorphism is powered by "subtyping", not by code inheritance.

   - Java's extends does "both at once"; C++ lets you choose; Java interfaces give you "subtyping without code reuse".
```

---

7. Read Composition vs. Inheritance: How to Choose?
```
    Prefer composition over inheritance when the relationship is HAS-A 
    or when you only want to reuse behaviour;
    use inheritance only for a genuine IS-A relationship, 
    because inheritance couples the subclass tightly to the superclass's implementation.
```

---

8. In the analysed code, identify testable methods and write a couple of unit tests for them (the IDE can help with this).

```

```

--- 

## 2) Static members (variables/constants and methods)

Analyse the source code in package lst01_04

### Exercises

---

1. Explain the following concepts:
    - static variable (field/class member)
    - static constant 
    - static method

```
    - A static variable (can be seen in Java as a class variable) is shared by all objects of the class
    - Static variables are initialized when class is loaded (so "only once" per class, before any object of that class is created)
    - static constants = static final variables; they are quite common (in contrast to mutable static variables)
```

---

2. Explain why static constants often have public visibility

```
A public static final constant is meant to be a shared, 
read-only value used by any other class (e.g., Math.PI, Integer.MAX_VALUE). 
Because it is final, no one can change it; 
because it is static, there is exactly one copy; 
because it is public, everyone can read it. 
This makes it a safe global constant,
unlike a public static mutable field, which would be a shared mutable global (bad).
```

---

3. Explain why static methods do not have access to instance members (methods and fields)

```
A static method is invoked on the class, so there is no this,
no particular object is associated with the call. 
Instance fields and instance methods only exist per object, 
so a static method has no object to read/write them from. 
This is why the compiler rejects m1(); 
inside a static block in HelloObjectInit.java:
```
``` java
// m1(); // Non-static method 'm1()' cannot be referenced from a static context
```

---

4. Give one example of a static method application

```
```

---

## 3) Constructors, factory methods, and singletons
   
Analyse the source code in packages:

```
- lst01_05 (object initialisation process)
- lst01_06 (order of constructor calls)
- lst01_07 (simple factory method)
- lst01_08 (singleton example implementations)
```

### Exercises

---

1. Describe the object initialisation process for a class derived from the Object class (including default values for different types of fields/variables, static variables, static constants, anonymous static blocks, anonymous blocks, constructors)

```
    1. All fields (static + instance) set to default values (0, 0.0, false, null).
    2. Static initialisers / static blocks run in textual order — once, at class load.
    3. On each new: instance field initialisers and instance blocks run in textual order.
    4. Constructor body runs (super()/this() first).
    5. Superclass initialisation happens before subclass initialisation.

    - static blocks are executed only once, when the program starts (the corresponding class is loaded); they are executed even if no instances are created</li>
    - normal blocks are executed whenever a new instance is created
```

---

2. For class D9 from (defined in ClassFamily.java):

   1. draw the class (inheritance) diagram

```
0. D9 -> D1 -> B1 -> Object : for inheritance

1. B1() → B1.B1()
2. D1() → D1.D1()
3. d7 = new D7(): B3(int) → D7(int) → D7()
4. d4 = new D4(): B3(int) → D4()
5. D9() body: D9.D9(), then d1 = new D1() → B1() → D1()
```

```
        Object
          |
         B1
          |
         D1
          |
         D9 ──composes──► D1 (field d1)
          ├──composes──► D4 (field d4)
          └──composes──► D7 (field d7)
```

   2. explain the sequence of the constructor calls

```
Constructor and Initialization Order
- The parent constructor is called before the child constructor.
- The fields and initialization blocks are initialized before the D9 constructor body is executed.
```

---

3. Compare capabilities of constructors and factory methods 

```
"Factory" methods(- a static method that returns new instances of the class), among other things, can:
- return an object of a subclass
- return a shared object, instead of unnecessarily constructing new ones
```

---

4. Give at least two applications of the singleton pattern

```
- Configuration/Properties Manager: one shared source of application settings.
- Logging Service: one centralized logger for the whole application.
- Caching: one shared in-memory cache.
- Thread Pool / Connection Pool: one shared pool of reusable resources.
- Hardware access: e.g., a printer spooler (the classic GoF example).
```

---

5. Write a couple of unit test (JUnit 5) for singletons from lst01_08

```java
class SingletonTest {
    @Test void eagerSameInstance() {
        assertSame(EagerSingleton.getInstance(), EagerSingleton.getInstance());
    }
    @Test void lazySameInstance() {
        assertSame(LazySingleton.getInstance(), LazySingleton.getInstance());
    }
    @Test void unsafeSameInstance() {
        assertSame(UnsafeSingleton.getInstance(), UnsafeSingleton.getInstance());
    }
    @Test void notNull() {
        assertNotNull(EagerSingleton.getInstance());
        assertNotNull(LazySingleton.getInstance());
    }
}
```

---

## 4) Immutable objects/classes and Java Records
   Analyse the source code in package lst01_09

### Exercises

---

1. Explain a strategy for defining immutable objects

```
The strategy for defining immutable objects is to look if its instances cannot be "mutated"

To define an immutable class:

1. Make the class final (prevent subclassing that could add mutable state).
2. Make all fields private and final.
3. Provide no setters.
4. Defensively copy mutable inputs in the constructor and mutable fields on return.
5. Do not let this escape during construction.
```

---

2. Compare the concepts of the immutable object and immutable class 
```
    An immutable object is implemented with a "final" modifier which make the field or the method a constant, and an immutable class make it closed for extension.

    In other words: 
        - An immutable object is a runtime notion: an object whose state cannot change after construction (e.g., a String instance).
        - An immutable class is a design notion: a class all of whose instances are immutable (e.g., String, Integer, BigDecimal, and Java records).

        - final on a field means the reference/value can't be reassigned necessary,but not sufficient for immutability (a final field can still point to a mutable object whose contents change).
        - final on a class means it can't be extended, a common part of the immutability recipe, but again not sufficient on its own.

    So: an immutable object is an instance; an immutable class is a class whose every instance is immutable.
```

---

3. Explain the advantages of immutable objects 

```
The advantages of Immutable objects are that they can be safely cached and reused, saving memory. 

Other notable pros are :
- Thread-safety without synchronisation 
- Safe sharing and caching
- No defensive copying on API boundaries 
- Simpler reasoning and fewer bugs
- Good building blocks for other immutable types 
- Failure atomicity
- Easy to test

```

---

4. Give at least two uses of the Java Records 

```
A Java Record is a special kind of Java class which has a concise syntax for defining immutable data-only classes. HelloJavaRecord corresponds to {@link HelloImmutable}
 
A record declaration specifies in a header a description of its contents.

The following class members are created automatically:
- the appropriate accessors (note a different naming convention not {@code  getVal()} but {@code val()})</li>
- constructor</li>
- equals method  
-hashCode method</li>
- toString method</li>
```

```
Record has at least two uses:

1. Immutable data carriers / DTOs: e.g., record Point(int x, int y) {} for coordinates, API responses, or value objects.
2. Compound map keys: records get correct equals/hashCode automatically, so they work safely as HashMap keys.
3. Returning multiple values from a method without writing a boilerplate class.
4. Pattern matching (Java 21+): records are commonly used as the target of record patterns.
```

---

5. Write a couple of unit tests to for HelloImmutable and HelloJavaRecord

```
```

---

## 5) Overriding hashCode, equals, and toString

   Analyse the source code in package lst01_10

### Exercises

---

1. Explain the difference between == operator and equals method in Java (consider primitive and reference types)

```
- For primitives, == compares values. There is no equals for primitives.
- For reference types, == compares references (same object in memory).
- equals (as defined by Object) defaults to reference equality, but classes like String, Integer, and records override it to compare state/content.
```

---

2. Explain the following formula o1.equals(o2) $\implies$ hasCode(o1) == hashCode(o2)

```
This is the equals/hashCode contract. 
If two objects are equal, they must have the same hash code. 
Otherwise hash-based collections (HashMap, HashSet) would fail to find them.

NB: the converse is not required: equal hash codes do not imply equality (that's a collision, 
which is allowed). The other direction of the contract is: if hashCode differs, the objects must not be equal.
```

---
3. Familiarize yourself with the Java Object class 

```
Object is the root of the class hierarchy. Every class implicitly extends it. Its key methods are:

- equals(Object), hashCode(), toString()
- getClass(), clone(), finalize() (deprecated)
- wait(), notify(), notifyAll()
```

---

4. Explain the general contract of hashCode and equals
```
equals contract (reflexive, symmetric, transitive, consistent, and x.equals(null) is false).

"hashCode" contract:

1. Consistent during one execution (same object → same hash, unless state used in equals changes).
2. o1.equals(o2) ⟹ o1.hashCode() == o2.hashCode().
3. Unequal objects may share a hash (collisions allowed).

NB : if you override equals, you must override hashCode.
```

---

5. Generate JavaDOC documentation for the project (hint: Tools > Generate JavaDoc)

```
```

---

## 6) Push the commits to the remote repository

---
