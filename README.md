# Unit 7: Advanced Object-Oriented Programming

## Introduction
Now that we've covered the basic object-oriented programming concepts in Unit 6, we can build on those and dive deeper into how this framework helps us take on more complicated tasks.

As always, use these concepts only when they clearly benefit your program. Pick the right tools for your situation.

### Table of Contents
- [Introduction](#introduction)
    - [Table of Contents](#table-of-contents)
- [Primitives vs. Non-Primitives](#primitives-vs-non-primitives)
    - [Default Values](#default-values)
    - [`null`](#null)
- [Java Files and Packages](#java-files-and-packages)
    - [Files](#files)
    - [Packages](#packages)
- [Inheritance](#inheritance)
    - [>Exercise: Botany Simulation](#exercise-botany-simulation)
- [Polymorphism](#polymorphism)
    - [Method Overriding](#method-overriding)
    - [Exercise: Ornithology](#exercise-ornithology)
- [Abstraction](#abstraction)
    - [Interfaces](#interfaces)
    - [>Exercise: Devices](#exercise-devices)
- [Recap](#recap)

[Feedback](#feedback) \
[License](#license)

## Primitives vs. Non-Primitives
<sub>(Slight detour)</sub>

In Java, types can be divided into two separate categories: **primitive** types and **non-primitive** types (also called reference types or object types).

**Primitive data types** are special, core data types built into Java. 
Java has 8 primitive data types: `byte`, `short`, `int`, `long`, `float`, `double`, `char`, and `boolean`. 

**Non-primitive data types**, or **reference types**, are types that refer to *objects* rather than primitives. All classes are reference types. \
Some examples of non-primitives include `String`, arrays, user-defined classes, and interfaces. \
(Internally, objects are stored in memory differently than primitives, which is why there is a distinction between them in Java and many other languages.)

The key differences between the two are:
- Primitive types are *not classes*, and their instances are *not OOP objects*; primitive values are more basic.
- So, primitive values do not have fields nor methods (with a few universal exceptions), and they aren't created with `new`.
- Primitive types start with a lowercase letter (E.g. `int`) while non-primitive types conventionally start with an uppercase letter (E.g. `String`).
- Reference type variables can take the value `null`.
- Using `==` on non-primitives compares if they are the same singular object, while `==` on primitives compares actual values. In general, use the `.equals()` method for non-primitives instead.

Every primitive type has a matching **wrapper** type, which is basically the primitive type made into a reference type. Sometimes, a type must be a reference type and not a primitive type, so this makes it possible to use primitive data in more places. \
Wrappers - `int`: `Integer`, `double`: `Double`, `boolean`: `Boolean`, `char`: `Character`, etc. \
Java will automatically convert between the two when necessary (e.g. in variable assignments). If you can, use the primitive version instead though.

### Default Values
Types have default values: when a field of the type isn't initially assigned anything, its value starts as the type's default value. \
(The default value is also used for arrays, taught in the next unit.)

The default value for all primitive types is **zero in the form of the type**. E.g., for `int`: `0`, `double`: `0.0`, `boolean`: `false`. \
The default value for all reference types is **`null`**.

### `null`
**`null`** is a special value that represents the *absence* of an object or reference. It essentially tells us that a variable is empty and doesn't refer to any object. \
Attempting to use methods or fields of an object that is actually null will result in an error while the program is running.

`null` can legally be used anywhere where a normal, non-primitive object is expected. So be cautious, as an unexpected `null` value is one of the most common reasons for a runtime crash. \

Many operations return `null` upon failure. \
Use `== null` to check for `null`.

```java
class Main {
    public static void main(String[] args) {
        String msg = null;
        System.out.println(msg == null); // true
        System.out.println(msg); // null
        System.out.println(msg.equalsIgnoreCase("Hello")); // ERROR!
        // Exception in thread "main" java.lang.NullPointerException:
        // Cannot invoke "String.equalsIgnoreCase(String)" because "<local1>" is null
    }
}
```

## Java Files and Packages
<sub>(Important detour)</sub>

As our programs get bigger and start to span many classes in many files, we need to organize them so they don't become one big mess. This is where **files** and **packages** come in!

### Files
We use Java files to write all of our Java programs! Any time we make an exercise or project, we do it in its own Java file (ends in `.java` file extension).

Each Java file stores one 'primary' class that *must be named the same as the file* (minus the `.java` file extension). Other classes can be declared in the file, too. <u>Only the primary class may be declared `public`</u> (if desired); all other classes declared in the file shouldn't have any access modifier.

VS Code will automatically adhere to these rules for you: new Java files are filled out with a matching empty public class declaration. And renaming the primary class automatically renames the file. \
(Technically, it is the Java extensions you installed at the start that are doing this.)

### Packages
**Packages** are groups of related Java files (and their classes), and correspond to *folders* (also called directories) in file systems.

**Libraries** are collections of related packages that you can use in your own programs so you don't have to write certain tools yourself.

Java itself provides the **standard library**, a *built-in* set of packages with many prewritten utility classes. E.g. `java.util` package provides the `Scanner` class and many others.

However, libraries are usually **third-party** (external, from neither Java itself nor from you). In this course, third-party libraries will not be used.

We can use packages by *importing* them, like so:
```java
// Do this at the top of the file, outside of any classes:
import package.name.Class; // Imports a single class from the package
import package.name.*; // Imports the whole package

// The rest of your program goes here
```
Here, we use the `import` keyword at the top of the Java file to indicate that we want to use a different class (part of a package!) in our current file. Then, we specify what package we want and how much we'll import (one class or the whole package?) by either specifying the class name or using `.*` to import everything in the package.

For example, when we want to use the `Scanner` class:
```java
import java.util.Scanner;
```

Or many things in the `java.util` package:
```java
import java.util.*;
```
Generally, you don't want to import an *entire* package unless you have to. It can clutter your program with unnecessary classes and names.

Your program is also made up of its own packages. \
A single folder corresponds to a single package. The Java files directly in that folder (not any subfolders) belong to that package. \
We can create our own package like this:
```java
package coolPackage;

public class Person {
    public void hi() {
        System.out.println("Hi!!");
    }
}
``` 
The `package` keyword in the first statement indicates that we are using the file to create a new package, and specifies the package name. The directory name and package name must match; the package structure must match the file structure.

Then, we can import our package in a different file to use it:
```java
import coolPackage.Person;

public class Main {
    public static void main(String[] args) {
        Person bob = new Person();
        bob.hi();
    }
}
```

Packages also affect access levels. From Unit 6, default and `protected` access levels are package-wide. \
All classes in the same exact package are accessible to each other without any need for importing.

VS Code will do most of the work for you with packages and imports: if you attempt to use a class then the import statement for it will be added automatically. The `package ...;` statement in files also gets provided by VS Code.

## Inheritance
**Inheritance** in Java allows one class to *inherit* the fields and methods of another. We refer to the class inheriting these properties the **subclass** (child), and the class being inherited from the **superclass** (parent, base). Inheritance allows us to reuse code from one class to another.

Conceptually, the subclass is usually more 'specific' or 'narrow', the superclass is more 'broad' or 'generic'. E.g. Dog would be a subclass of Animal. A dog has everything an animal has, and even more. \
So when we declare the Dog class, we don't have to start from scratch; we can just add on to whatever was in the Animal class.

A class can only inherit from a single class. One class may have many subclasses though. \
A tree graph ("inheritance tree") or hierarchy is often used to visualize inheritance.

The `extends` keyword is used in the subclass declaration to indicate inheritance. \
The `super` keyword is like the `this` keyword, but it accesses members from the superclass instead of the subclass. \
Here is what inheritance looks like in code:
```java
class Book { // Superclass
    // Fields that will be inherited by a subclass
    public String title;
    public String author;
    public int pageNumber = 1;

    public Book(String title, String author) {
        this.title = title;
        this.author = author;
    }

    // Method that will be inherited by a subclass
    public void turnPage() {
        pageNumber++;
        System.out.println("Turned to page " + pageNumber);
    }
}
```

```java
class GraphicNovel extends Book { // Subclass
    // A field unique to graphic novels
    public String illustrator;

    public GraphicNovel(String title, String author, String illustrator) {
        // Calls the Book constructor to initialize these values
        super(title, author);
        // Illustrator must be initialized on its own
        this.illustrator = illustrator;
    }

    public static void main(String[] args) {
        GraphicNovel spiderman = new GraphicNovel("Spiderman", "Joe Kelly", "John Romita Jr. and Pepe Larraz");
        spiderman.turnPage();
        System.out.println(spiderman.author);
        System.out.println(spiderman.illustrator);
    }
}
```
Output:
```
Turned to page 2
Joe Kelly
John Romita Jr. and Pepe Larraz
```

In this example, our parent or *superclass* is `Book`. `Book` defines the fields `title`, `author`, and `pageNumber`, a constructor that initializes these fields, as well as the method `turnPage()`.

Our child or *subclass* is `GraphicNovel`. `GraphicNovel` inherits members from book by using the `extends` keyword when the subclass is declared (`class GraphicNovel extends Book`). So `GraphicNovel` can use all the fields and methods we declared in `Book`.

Additionally, `GraphicNovel` adds a field of its own, `illustrator`. Note that `Book` does *not* inherit this field from `GraphicNovel`; only `GraphicNovel` inherits members from `Book`.

In the `GraphicNovel` constructor, we use the `super` keyword. By using the parentheses `()` after `super`, the constructor of the parent class gets called. In this case, we use it to initialize the `title` and `author` fields of our graphic novel, because their initialization doesn't change from `Book` to `GraphicNovel`. We do this by calling `super()` in the constructor, and passing in the necessary parameters. Since the `illustrator` field was not inherited from `Book`, we must initialize it independently in the subclass constructor.

In the `main()` method, we can see that `GraphicNovel` objects are able to use methods and fields from `Book` freely, as well as properties of the `GraphicNovel` class.

> **Note:** There are extra rules for constructors of subclasses. A subclass constructor must call the superclass constructor as the very first thing it does. (If the call is omitted, then the parameterless superclass constructor is implicitly called.)

### >Exercise: Botany Simulation
You're a botany student trying to create a simulation of how different plant species behave throughout their life cycles. You're most interested in the behaviors of non-carnivorous plants vs. carnivorous plants. First, you have to set up classes for both that define what traits and behaviors will be used.

[`Plant.java`](<Botany Simulation/Plant.java>) [`CarnivorousPlant.java`](<Botany Simulation/CarnivorousPlant.java>)
1. Inside `Plant.java`, create a class that will define general behaviors and traits of plants. Add fields for `age`, `height`, and `color`, as well as a parameterized constructor to initialize these fields. Add two methods called `drinkWater()` and `photosynthesize()` that print what the plant is doing.
2. Inside `CarnivorousPlant.java`, create a class that inherits from the `Plant` class.
3. Add a method in the `CarnivorousPlant` class, `eatInsect()`, that prints what the plant is doing.
4. Add a constructor for the `CarnivorousPlant` class that just invokes the `Plant` constructor with the same parameters.
5. Inside the `main()` method, create a new `CarnivorousPlant` called `venusFlytrap`. The plant is 3 years old, 12cm tall, and light green in color.
6. Test your `drinkWater()`, `photosynthesize()`, and `eatInsect()` methods. You now have a very healthy simulated venus flytrap!

<details><summary>Solution Code</summary>

Plant.java:

```java
public class Plant {
    int age;
    double height;
    String color;

    public Plant(int age, double height, String color) {
        this.age = age;
        this.height = height;
        this.color = color;
    }

    public void drinkWater() {
        System.out.println("The plant is drinking water.");
    }

    public void photosyntesize() {
        System.out.println("The plant is photosynthesizing.");
    }
}
```

CarnivorousPlant.java

```java
public class CarnivorousPlant extends Plant {
    public void eatInsect() {
        System.out.println("The plant is eating an insect.");
    }

    public CarnivorousPlant(int age, double height, String color) {
        super(age, height, color);
    }

    public static void main(String[] args) {
        CarnivorousPlant venusFlytrap = new CarnivorousPlant(3, 12.0, "light green");
        venusFlytrap.drinkWater();
        venusFlytrap.photosyntesize();
        venusFlytrap.eatInsect();
    }
}
```

Output:
```
The plant is drinking water.
The plant is photosynthesizing.
The plant is eating an insect.
```
</details>

## Polymorphism
In Java, the *declared type* (type listed in the variable declaration or method return type) and the *actual type* (type of the actual object) can differ in some cases. \
A subclass object (actual type) can be used anywhere where the parent class is declared (declared type). The reverse is not true.
Example: if `Dog` extends `Animal`:
```java
Animal myAnimal = new Animal(); // Okay
Dog myDog = new Dog(); // Okay

Animal yourAnimal = new Dog(); // OKAY: Dog is also an Animal
Dog yourDog = new Animal(); // ERROR! An animal is not a dog!
```
This ability to use a subclass object in place of a superclass object is called **polymorphism**, and it is very useful. \
What the variable *may do* legally depends on the declared type (so `yourAnimal` is restricted to only `Animal` methods, even though it is a `Dog`). \
But what the variable *actually does* depends on the actual type, and the behavior may be different if the subclass **overrides** methods in the superclass.

This is not the same as *overloading*. We covered method overloading already in Unit 5, but just to review: \
**Method overloading** is when we define multiple methods with the *same name* but *different parameters* (number, type, order).

### Method Overriding
**Method overriding** allows child classes to redefine the methods of parent classes. \
The following must stay the same in the redefined method:
- Method name
- Parameters
- Return type

(If you changed the parameters, it would be an overload instead of an override.)

You should also add the `@Override` **annotation** on the line before the method declaration, though this is optional (purely as a safeguard). \
Method override example:
```java
public class Cat {
    public void makeSound() {
        System.out.println("Meow");
    }
}
```
```java
public class Lion extends Cat {
    @Override
    public void makeSound() {
        System.out.println("ROAR!!");
    }

    public static void main(String[] args) {
        Cat cuteCat = new Cat();
        cuteCat.makeSound();

        Lion bigLion = new Lion();
        bigLion.makeSound();

        cuteCat = new Lion();
        cuteCat.makeSound();
    }
}
```
Output:
```
Meow
ROAR!!
ROAR!!
```
In the `Lion` class, we override the inherited `makeSound()` method and change its functionality (keeping the name, parameters, and return type the same!). \
So when the `makeSound()` method is called, it is `Meow` for `Cat` objects and `ROAR!!` for `Lion` objects, regardless of the declared type of the variable.

The `super` keyword can be used to call the overridden method from the newer method, helpful when you want to *extend* behavior rather than completely *replace* it.

### >Exercise: Ornithology
You're an ornithologist (you study birds). You want to make a video game that will help elementary school students learn the different types of birds, starting with whether or not a species is capable of flight.

[`Orinthology.java`](Orinthology/Bird.java) [`FlightlessBird.java`](Orinthology/FlightlessBird.java)
1. In `Bird.java`, create a class with a single method `fly()` that prints `I'm flying!`.
2. In `FlightlessBird.java`, create a class that inherits the `fly()` method from `Bird.java`.
3. *Override* this method to print `I can't fly :(` instead of `I'm flying!`.
4. In the `main` method of `FlightlessBird.java`, create two new birds. One `Bird` called `crow`, and one `FlightlessBird` called `penguin`.
5. Call the `fly()` method on both. `crow.fly()` should print `I'm flying!`, while `penguin.fly()` should print `I can't fly :(`

<details><summary>Solution Code</summary>

Bird.java:

```java
public class Bird {
    public void fly() {
        System.out.println("I'm flying!");
    }
}
```

FlightlessBird.java:

```java
public class FlightlessBird extends Bird {
    @Override
    public void fly() {
        System.out.println("I can't fly :(");
    }

    public static void main (String[] args) {
        Bird crow = new Bird();
        FlightlessBird penguin = new FlightlessBird();
        crow.fly();
        penguin.fly();
    }
}
```

Output:
```
I'm flying!
I can't fly :(
```
</details>

## Abstraction
**Abstraction** is a broad concept in programming, referring to the act of hiding details and only displaying the essential features of something. \
It lets us focus on *what* something is doing rather than exactly *how*.

Methods are a common example. For example, you've used `System.out.println()`: you don't know precisely how Java executes it, but you do know that it gets text on screen and that's what's important. \

Two advanced OOP tools for abstraction are **abstract classes** and **interfaces**, but we will not cover abstract classes in this course.

### Interfaces
**Interfaces** are a way for us to define just the exposed parts of a class (e.g. public fields, definitions of public methods) without any actual implementation (executable code). \
We can use interfaces to define *blueprints* for classes, declaring what they should be able to do but not the exact procedure.

Interfaces look like this (use `interface` instead of `class`):
```java
interface Car {
    void drive();
    void honk();
}

class Minivan implements Car {
    public void drive() {
        System.out.println("Driving a minivan!");
    }
    
    public void honk() {
        System.out.println("HONK!");
    }

    public static void main(String[] args) {
        Car van = new Minivan();
        van.drive();
        van.honk();
    }
}
```
Output:
```
Driving a minivan!
HONK!
```

In the `Car` interface, we define methods to be implemented by classes, but do not specify the contents of any method bodies - by default, these methods are *abstract*. Specific functionality is left to the classes, not the interface.

Interfaces can actually still have **concrete** methods (have a body and implementation) which get inherited, but the abstract methods are just bare declarations that end in a semicolon `;` instead of a code body.

In the `Minivan` class, we use this interface by writing the `implements` keyword when we create the class (`class Minivan implements Car`). This tells us that the `Minivan` class will implement the abstract methods defined in `Car` and give them an actual method body and functionality. Once these methods are fully defined in `Minivan`, objects of `Minivan` can call them freely.

We can still use an interface as a declared type. No object ever has its actual type be an interface, but as we've seen with polymorphism, other objects of valid types can be used.

Most interfaces you'll encounter are very small and have generic blanket names like `Runnable` or `Supplier`.

### >Exercise: Devices
You're an electrician creating a model for different household electrical appliances. You plan to give all of them the ability to turn on and turn off, but want to leave the specific functionalities for later.

[`Device.java`](Devices/Device.java) [`LightSwitch.java`](Devices/LightSwitch.java) [`Computer.java`](Devices/Computer.java)
1. In `Device.java`, create an interface class for your devices that outlines two `void` methods, `turnOn()` and `turnOff()`.
2. In `LightSwitch.java`, create a class that uses this blueprint.
3. Add a field in the `LightSwitch` class that tracks whether the light is on or off (starting at off).
4. Define method bodies for `turnOn()` and `turnOff()` that print their respective actions and update the on/off field.
5. In a `main()` method in the `LightSwitch` class, create a new `LightSwitch`. Try turning on and off, and print whether it is on/off to check the accuracy of the field value.
6. In `Computer.java`, create a class that uses your `Device` blueprint as well.
7. Create a field to track whether the computer is on or off (starting at off)
8. Define the `turnOn()` and `turnOff()` methods to print "Hello World!" and "Goodbye World!" respectively, and to update your on/off variable.
9. Add an additional `type()` method that takes in a `String` and, if the computer is on, prints it out. If not, it does nothing.
10. In a `main()` method in the `Computer` class, make a new computer and test it out! Try typing something, turn it on, type something, and turn it off. Make sure the on/off field is being updated correctly!

<details><summary>Solution Code</summary>

Device.java:
```java
interface Device {
    void turnOn();
    void turnOff();
}
```

LightSwitch.java:
```java
public class LightSwitch implements Device {
    boolean isOn = false;

    public void turnOn() {
        isOn = true;
        System.out.println("Light on!");
    }

    public void turnOff() {
        isOn = false;
        System.out.println("Light off!");
    }

    public static void main (String[] args) {
        LightSwitch light = new LightSwitch();
        light.turnOn();
        System.out.println(light.isOn);
        light.turnOff();
        System.out.println(light.isOn);
    }
}
```

Computer.java:
```java
public class Computer implements Device {
    boolean isOn = false;

    public void turnOn() {
        isOn = true;
        System.out.println("Hello World!");
    }

    public void turnOff() {
        isOn = false;
        System.out.println("Goodbye World!");
    }

    public void type(String message) {
        if (isOn) {
        System.out.println(message);
        }
    }

    public static void main(String [] args) {
        Computer laptop = new Computer();
        laptop.type("asdfgjm");
        laptop.turnOn();
        laptop.type("asdfgjm");
        laptop.turnOff();
    }
}
```

Output:

Light Switch:
```
Light on!
true
Light off!
false
```

Computer:
```
Hello World!
asdfgjm
Goodbye World!
```
</details>

## Recap
- Types are either primitive (`int`, `boolean`) or non-primitive (`String`, all classes)
- Primitive types each have a corresponding non-primitive wrapper type to allow usage in certain places
- `null` is a special value that can be in place of any non-primitive object; common cause of errors
- A Java file contains classes; one class must match the file name, and only it may be a public class
- Packages are collections of related files, and are defined with `package` statement; the package structure must match the file structure
- Use `import` to import public classes from packages (VS Code will do this automatically)
- Libraries are collections of packages; Java provides the standard library, and there are many third-party libraries
- Using inheritance, a class can be a subclass (child) of a superclass (parent); use `extends` in the subclass declaration, e.g.: `class Dog extends Animal { ... }`
- The `super` keyword is like `this` but the members are from the superclass instead
- With polymorphism, the declared type (variable, return type) can accept objects of subclasses too; declared type dictates what is possible/legal, actual object type governs behavior
- Method overriding occurs when a subclass defines a method with the same name as a superclass method, to replace or extend its behavior
- Interfaces, defined with `interface` keyword instead of `class`, define the exposed parts (implementation is optional) of what an adhering class has

## Feedback
Please provide feedback if you have any.
<details><summary>Possible feedback points</summary>

- Confusing explanations
- Knowledge, skills, or procedures that were required but weren't taught
- Too fast or too slow explanations; pacing
- Too hard or too easy exercises
- Step-by-step instructions that are not comprehensive/thorough enough, or didn't work
- Seemingly unnecessary or ineffective information or exercises
- Any improvements (e.g. wording) or more effective ways/formats to teach
</details>

___



## License
© 2025 @aatle, @spacepotatoes3, @gjgarson, @KaitoTLex

This work is licensed under a [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).
