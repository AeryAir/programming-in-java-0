# Java - Elements of Object-Oriented Programming (Part I)
## 1. Concepts of Encapsulation, Inheritance, and Polymorphism

### Exercises

---

1. **Explain the concept of encapsulation and the way it is implemented in Java.**

**Encapsulation** is often described as one or more of the following:

```
    - Bundling together publicly accessible methods and privately accessible data.
    - Data hiding (a controversial definition — see the linked discussion below).
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
    - The "this" keyword refers to the current object's own private roster of variables (and can also be used to call another constructor of the same class).

    - The "super" keyword refers to the inherited (parent) class's own private roster of variables and methods (and can be used to call a parent class constructor).
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

- **Ad-hoc polymorphism** (overloading of operators, functions, or — as in Java — methods).

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

   Reference to be added.

---

8. In the analysed code, identify testable methods and write a couple of unit tests for them (the IDE can help with this).

   Exercise to be completed based on the analysed code.

--- 

## 2) Static members (variables/constants and methods)

Analyse the source code in package lst01_04

### Exercises

1. Explain the following concepts:
    - static variable (field/class member)
    - static constant 
    - static method


2. Explain why static constants often have public visibility 
3. Explain why static methods do not have access to instance members (methods and fields)
4. Give one example of a static method application
