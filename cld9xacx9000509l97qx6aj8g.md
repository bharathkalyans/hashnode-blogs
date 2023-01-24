# Abstraction (In Depth)🥶

### What is Abstraction?

* In simple terms, abstraction refers to the process of hiding the implementation details of a class and showing only the necessary information to the user.
    

### How to Implement Abstraction?

* There are 2 ways in Java in which you can implement abstraction.
    
    * Abstract Classes
        
    * Interfaces
        

### Abstract Classes

* `` `abstract` `` keyword is used to create an Abstract Class.
    
* These classes cannot be used to create an Object.
    

```java
abstract class Shape {
    abstract double getArea();
}
public class Sample {
    public static void main(String[] args) {
        Shape object = new Shape(); ❌
    }
}
```

* We must extend this abstract class to a child class and only then will we be able to create an object.
    

```java
abstract class Shape {
    abstract double getArea();
}

class Square extends Shape {
    double side;

    Square(double side) {
        this.side = side;
    }

    double getArea() {
        return side * side;
    }
}

public class Sample {
    public static void main(String[] args) {
        Shape squareOne = new Square(12); ✅
        Square squareTwo = new Square(13); ✅
    }

}
```

* Abstract classes can have data members and the methods in these classes can have the functionality implemented.
    

```java
abstract class Shape {
    private int diameter = 12;

    void printDiameter() {
        System.out.println("Diameter is :: " + this.diameter);
    }

    abstract double getArea();
}
```

### Interfaces

* Interfaces are a way to define a contract for a class to implement.
    

```java
interface Shape {
    double getArea();   
}

class Square implements Shape {
    double side;

    Square(double side) {
        this.side = side;
    }

    @Override
    public double getArea() {
        return side * side;
    }
}


public class Sample {
    public static void main(String[] args) {
        Shape squareOne = new Square(10); ✅
        Square squareTwo = new Square(10); ✅

        System.out.println(squareOne.getArea());
        System.out.println(squareTwo.getArea());
    }

}
```

* As in the case of abstract classes you cannot create objects of interfaces.
    

```java
interface Shape {
    double getArea();
}
public class Sample {
    public static void main(String[] args) {
        Shape shape = new Shape(100); ❌
    }
}
```

### Key Differences between Abstract Class and Interface

* Abstract classes can have both abstract and non-abstract methods, while all methods in an interface are abstract by default.
    
* Abstract classes can have constructors, but interfaces cannot.
    
* A class can extend only one abstract class, but it can implement multiple interfaces.
    
* Abstract classes can have instance variables, while interfaces cannot.\*
    
* Abstract classes can provide a default implementation for an interface, while interfaces cannot.\*
    
* An abstract class can have non-final variables, while variables in an interface are final by default.
    
* An abstract class can provide a standard implementation for its subclasses, while an interface cannot provide any implementation for its implementing classes.\*
    
* But after the introduction of `JDK 8` there were a couple of changes introduced to interfaces.
    
* Interfaces can have declared variables that are `static` and `final` by default.
    

```java
interface Shape {
    int diameter = 0; ✅
    double getArea();
}
public class Sample {
    public static void main(String[] args) {
        System.out.println(Shape.diameter);
    }
}
```

* Interfaces now can have functionality defined to the methods using the `default` keyword. Subclasses can implement their own functionality of these already defined methods but shouldn't be using `@Override` keyword.
    

```java
interface Shape {

    double getArea();

    default void print() { 
        System.out.println("Printing!");
    } ✅

    default float getPIValue() {
        return 3.1432f;
    } ✅
}
```

### When to use Abstract Class and When to use Interfaces?

* Abstract classes are meant to provide a common implementation for subclasses, while interfaces are meant to define a contract for classes to implement.
    
* In most cases, developers will be using interfaces rather than abstract classes because Java doesn't support multiple inheritances, it only supports multi-level inheritance.
    

```java
//Common Implementation among all the MacBook laptops.
abstract class MacBook {
    abstract void loadMacOS();
}

interface CoolingSystem {
    boolean hasThermalSystem();

    boolean hasFanSystem();
}

class MacBookAir extends MacBook implements CoolingSystem {

    @Override
    void loadMacOS() {
        System.out.println("Loading MacOS Centura 1.2v");
    }

    @Override
    public boolean hasThermalSystem() {
        return true;
    }

    @Override
    public boolean hasFanSystem() {
        return false;
    }
}

class MacBookPro extends MacBook implements CoolingSystem {

    @Override
    void loadMacOS() {
        System.out.println("Loading MacOS Centura 1.2v");
    }

    @Override
    public boolean hasThermalSystem() {
        return true;
    }

    @Override
    public boolean hasFanSystem() {
        return true;
    }
}

public class Sample {
    public static void main(String[] args) {
        MacBookAir macBookAir = new MacBookAir();
        MacBookPro macBookPro = new MacBookPro();

        System.out.println(macBookAir.hasFanSystem());
        System.out.println(macBookAir.hasThermalSystem());

        System.out.println(macBookPro.hasFanSystem());
        System.out.println(macBookPro.hasThermalSystem());
    }
}
```

* Above is a simple and concise example of how abstract classes and interfaces are used together.
    

---

Hope you liked this blog!

Connect with me @[twitter](https://twitter.com/bharathkalyans).