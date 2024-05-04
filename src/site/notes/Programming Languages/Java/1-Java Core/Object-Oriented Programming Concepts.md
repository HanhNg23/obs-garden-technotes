---
{"dg-publish":true,"permalink":"/Programming Languages/Java/1-Java Core/Object-Oriented Programming Concepts/","title":"Object-Oriented Programming Core Concepts","noteIcon":"1","updated":"2024-05-04T13:49:48.005+07:00"}
---


<span style="color:#6a5858; font-size: 85%;">Object-oriented programming (OOP) is a programming paradigm that organizes code around the concept of "objects", which are instances of classes that encapsulate data and procedures. OOP promotes concept such as encapsulation, inheritance, and polymorphism, making code more modular, reusable, and flexible for easier management of complex systems. In this post, we are going to explore the key concepts used in OOP.</span>

## Object
- <span style="color:#537288">Real-word objects</span>
	- Everything arounds you, exists in nature
	- Share 2 characteristics: <span style="color:#9a7db0">State And Behavior</span>
	- Example: 
		- Dogs have state (name, color, breed, hungry) and behavior (barking, fetching, wagging tail)
		- Bicycles have state (current gear, current pedal cadence, current speed) and behavior (changing gear, changing pedal cadence, applying brakes)

- <span style="color:#537288">Software objects</span>
	- Conceptually similar to Real-world objects
	- Consist of state and related behavior
	- An object stores its <span style="color:#9a7db0">state in fields</span> (variables in some programming languages) and exposes its <span style="color:#9a7db0">behavior through methods</span> (functions in some programming languages)
	- Objects communication: methods of objects operate on an object's internal state (such as get(), set() method) and serve as the primary mechanism for object-to-object communication --> The action of <span style="color:#9a7db0">hiding internal state</span> and requiring all <span style="color:#9a7db0">objects' interactions to be performed through an object's methods</span> - <span style="color:#9a7db0">known as "**Data Encapsulation**"</span> - a fundamental <span style="color:#9a7db0">principle of object-oriented programming</span>
	- A software object overview
		- ![](https://docs.oracle.com/javase/tutorial/figures/java/concepts-object.gif)
	- A software bicycle object example:
		- ![](https://docs.oracle.com/javase/tutorial/figures/java/concepts-bicycleObject.gif)
- <span style="color:#537288">Benefits of organizing code into separate software objects offers numerous benefits</span>
	- Modularity: allows for independent development and integration of object code
	- Information-hiding: ensures that internal implementation details are kept private
	- Core re-use: permits the utilization of pre-existing objects in new programs, enhancing efficiency and reliability when implement/test/debug, task-specific objects.
	- Pluggability & Debugging ease: facilitate the removal and replacement of problematic objects without affecting the entire system, akin to fixing mechanical components in the real world.

## Class
- <span style="color:#537288">Given the situation:</span> in the real world, it is common to come across multiple individual objects of the same kind.
	- *For example, there may be thousands of other bicycles in existence, all of the same make and model - Each bicycle was built from the same set of blueprints and therefore contains the same components.*
	- <span style="color:#9a7db0"> --> In object-oriented terms, your bicycle is an instance of the class of objects known as bicycles.</span>
- <span style="color:#d4a216">==> A class is the blueprint from which individual objects are created.</span>
- ![](https://i.imgur.com/YYShCg5.png)

	```Java
	class Bicycle {
	
	    int cadence = 0;
	    int speed = 0;
	    int gear = 1;
	
	    void changeCadence(int newValue) {
	         cadence = newValue;
	    }
	
	    void changeGear(int newValue) {
	         gear = newValue;
	    }
	
	    void speedUp(int increment) {
	         speed = speed + increment;   
	    }
	
	    void applyBrakes(int decrement) {
	         speed = speed - decrement;
	    }
	
	    void printStates() {
	         System.out.println("cadence:" +
	             cadence + " speed:" + 
	             speed + " gear:" + gear);
	    }
	}
	
	```
- The **Bicycle class** here is the **blueprint** which is used in the application - it means that some other class in your application will use this blueprint to new Bicycle objects.
	- For example, a **BicycleDemo class** creates two seperate **Bicyle objects** and invokes their methods:
	```Java
	class BicycleDemo {
	    public static void main(String[] args) {
	
	        // Create two different 
	        // Bicycle objects
	        Bicycle bike1 = new Bicycle();
	        Bicycle bike2 = new Bicycle();
	
	        // Invoke methods on 
	        // those objects
	        bike1.changeCadence(50);
	        bike1.speedUp(10);
	        bike1.changeGear(2);
	        bike1.printStates();
	
	        bike2.changeCadence(50);
	        bike2.speedUp(10);
	        bike2.changeGear(2);
	        bike2.changeCadence(40);
	        bike2.speedUp(10);
	        bike2.changeGear(3);
	        bike2.printStates();
	    }
	}
		
	```

## Inheritance
- <span style="color:#537288">Given the situation:</span> Different types of objects frequently share similarities or characteristics with one another.
	- For example:
		- Mountain bikes, road bikes, and tandem bikes -- these all share the characteristics of bicycles (current speed, current pedal cadence, current gear)
		- --> But problem here, that is although they have many in commons but each kind of them defines additional features that make them different: 
			- *tandem bicycles have two seats and two sets of handlebars* 
			- *road bikes have drop handlebars* 
			- *some mountain bikes have an additional chain ring, giving them a lower gear ratio*
- <span style="color:#537288">Resolves problem:</span> <span style="color:#9a7db0">Object-oriented programming allows classes to _inherit_ commonly used state and behavior from other classes</span>
	- For example, `Bicycle` now becomes the _superclass_ of `MountainBike`, `RoadBike`, and `TandemBike`
	- ![](https://docs.oracle.com/javase/tutorial/figures/java/concepts-bikeHierarchy.gif)

> [!WARNING]
> In the Java programming language, <span style="color:#d4a216">**EACH CLASS IS ALLOWED TO HAVE ONE DIRECT SUPERCLASS**</span>. <span style="color:#9a7db0">But each superclass has the potential for an unlimited number of subclasses</span>
- The syntax for creating a subclass example:
```Java
	class MountainBike **extends** Bicycle {
	
    // new fields and methods defining 
    // a mountain bike would go here
	
}
//This gives `MountainBike` all the same fields and methods as `Bicycle`, yet allows its code to focus exclusively on the features that make it unique
```

> [!INFO] References to Java Inheritance
> - 🍎 [Inheritance in Java](https://www.geeksforgeeks.org/inheritance-in-java/)

## Interface
- As we have already mentioned, <span style="color:#9a7db0">***objects communicate with the outside world through the methods that they expose***</span>.
- Methods serve as the means with the external environment.
	- For example, the ***buttons*** on the front of your TV set - are the ***interface between you and the electrical wiring on the other side of its plastic casing*** --> If you press the "power button" --> the television turns on or off.
- <span style="color:#d4a216">The interface typical form is a collection of related methods that do not have any implementation details == methods with empty body.</span>
	- For example, describe bicycle's behavior as an interface:
	```Java
	interface Bicycle {
	
	    //  wheel revolutions per minute
	    void changeCadence(int newValue);
	
	    void changeGear(int newValue);
	
	    void speedUp(int increment);
	
	    void applyBrakes(int decrement);
	}
	```
	- A new class will implement this interface using the `implements` keyword. The implemented classes of this interface must override the methods defined in the interface.
	```Java
	class ACMEBicycle **implements** Bicycle {
	
	    int cadence = 0;
	    int speed = 0;
	    int gear = 1;
	
	   // The compiler will now require that methods
	   // changeCadence, changeGear, speedUp, and applyBrakes
	   // all be implemented. Compilation will fail if those
	   // methods are missing from this class.
	
	    void changeCadence(int newValue) {
	         cadence = newValue;
	    }
	
	    void changeGear(int newValue) {
	         gear = newValue;
	    }
	
	    void speedUp(int increment) {
	         speed = speed + increment;   
	    }
	
	    void applyBrakes(int decrement) {
	         speed = speed - decrement;
	    }
	
	    void printStates() {
	         System.out.println("cadence:" +
	             cadence + " speed:" + 
	             speed + " gear:" + gear);
	    }
	}	
	```

- <span style="color:#d4a216">The powerful of implementing Interface</span>
	 - Implementing an interface <span style="color:#d4a216">allows a class to become more formal about the behavior it promises to provide</span>.
	 - Interfaces <span style="color:#d4a216">form a contract between the class and the outside world</span>. This contract is enforced at build time by the compiler --> If your ***class claims to implement an interface***, ***all methods defined by that interface must appear in its source code*** before the class will successfully compile.

## Package
- <span style="color:#9a7db0">A package</span> - <span style="color:#9a7db0">a namespace that organizes a set of related classes and interfaces.</span>
	- *<span style="color:#6a5858">Similar to your different folders on your computer. You might keep HTML pages in one folder, images in another, and scripts or applications in yet another for example.</span>*
- <span style="color:#9a7db0">The Java platform provides an enormous class library (a set of packages) </span> suitable for use in your own applications. 
	- <span style="color:#9a7db0">This library is known as the **"Application Programming Interface"**, or **"API"** for short</span>
		- *Developers can create their own libraries and compress as file jar for reusing across java software projects.*
	- In Java, there are two types of packages: built-in packages and user-defined packages.
		- Built-in Packages: predefined packages known as standard packages of the Java Development Kit (JDK), support a wide range of classes and interfaces for various purpose.
			- Some commonly used built-in packages include: `java.lang`, `java.util`, `java.io` and so on.
			- ![](https://qph.cf2.quoracdn.net/main-qimg-1ad4e431274c1ce4ed2c0abd297350a1)
		- User-defined packages: packages created by Java developers to organize their classes and resources, improving code organization, readability, and maintainability. 
			- [![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjIkD5SgSzxfqkHgLfMlvRuigEK5Lc4wDHIHQ6HFOTR86GaoMmPybBhWnSYabYxiAjN3eZRhI7tK7vglYsZvNTilc0rmrWVXo_hLBJUB6_2YbWmxWT16Fw-7Z9ACxgNDbggFq_cIGY1XVA/s400/Package+tutorial+in+Java.png)](https://www.java67.com/2018/06/a-beginners-guide-to-package-in-java.html)
	- Its packages represent the tasks most commonly associated with general-purpose programming. 
		- <span style="color:#6a5858">For example:</span>
			- <span style="color:#6a5858"> a `String` object contains state and behavior for character strings;</span> 
			- <span style="color:#6a5858">a `File` object allows a programmer to easily create, delete, inspect, compare, or modify a file on the filesystem;</span>
## References
- [Object-Oriented Programming Concepts](https://docs.oracle.com/javase/tutorial/java/concepts/index.html)