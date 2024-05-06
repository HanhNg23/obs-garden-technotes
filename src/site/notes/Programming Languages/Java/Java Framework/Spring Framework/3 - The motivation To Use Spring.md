---
{"dg-publish":true,"permalink":"/Programming Languages/Java/Java Framework/Spring Framework/3 - The motivation To Use Spring/","title":"Spring IoC Container Manages your Spring Beans","noteIcon":"1","updated":"2024-05-06T19:10:29.968+07:00"}
---


<div style="color:#6a5858; font-size: 85%;">
<p>👁️ The Spring Container is the core of the Spring Framework and is responsible for managing the lifecycle of Java objects, also known as beans. </p>
<p> 👁️ It is a lightweight, non-invasive, and flexible container that provides support for dependency injection, aspect-oriented programming, and other features that help developers build robust and scalable applications.</p>
<p> 👁️ The Spring Container is at the heart of the Spring Framework and plays a crucial role in simplifying the development process by handling the creation, configuration, and management of beans.</p>
<p> 👉👉 In here, i will cover the Spring Inversion of Control (IoC) and how it manages your spring beans. </p>
</div>

## Dependencies Issue ?
> - <span style="color:#6a5858">In Java Application, different objects work together i.e., objects collaborate to accomplish a goal and subsequently have dependencies.</span>
- To control the coded dependencies couple objects to each other directly **--> We use the Spring framework, it can help avoid such brittle dependencies**.
- The Spring framework achieves loose coupling through Interfaces.
- Example
	- <span style="color:#6a5858">We will use the simplistic example to illustrate how the coupling through interfaces through this post. The example consists of a Service delegating to a Data Access Object (DAO) and we start with two interfaces to define our domain context.</span>
	- ![](https://i.imgur.com/0uPSC1v.png)

## Reduce Coupling between Dependent Classes
### Tight Coupling Issue
- ![](https://i.imgur.com/ZUsX7su.png)
- In implementation classes, the problem is the tight coupling in the `getDao()` method. Our Service implementation is tied directly to an implementation class.
- There is a dependency is in the code, the `EmployeeService` depends on the `EmployeeDao` (the implementation of `EmployeDao` is regard as a dependency) -- which result in the issue --> it will require a code change if another `EmployeeDao` implementation class is required later to accommodate the requirement change.
	- For example, if the `EmployeeService` require to use other specific `EmployeeDao` implementation like `EmployeeDaoImpl` B or C or D --> but because of the dependency between Service and Dao, so there is a compulsory to change the type implementation of `EmployeeDao`.
### Reduce Coupling Solution
- 🧐 What if we reduce coupling via Factory
	- ![](https://i.imgur.com/aPbWzN1.png)
		- The `getDao` method now delegates to an `employeeDaoFactory` which has a method `getInstance` which will return type of `getDao` method - return the type `EmployeeDao` 
		- 🤔 --> So the decision as to "what DAO will be used by our Service" is given and encapsulated by another class (in here is `EmployeeDaoFactory`).
		- 🙄 However, the client class (`EmployeeService`) STILL has to look up the dependency object itself from the Factory. In the case we changed the implementation of the `getInstance` method and `employeeDaoFactory`, we will have to recompile again.
	
- 🧐 Can someone else do the "instantiation" of my Classes and inject their dependencies to reduce coupling ?
	- ![](https://i.imgur.com/4hoapLq.png)
		- Another class create the DAO and give it to our Service class instance via a setter
	- ![](https://i.imgur.com/yrzKcul.png)
		- 🤔 So our code in our Service class now is very clean. However, our client class, mean the main class above, appears to be working heavily. 🧐 It needs to know the intimate details of the Service such that it has a `EmployeeDaoImpl`.
			- <span style="color:#878282">From the example above, the client has to create the Service instance, has to create the `EmployeeDao` instance and then execute the set method. It means the client has intimate knowledge of the dependencies of `EmployeeService` - has to know what are the dependencies of `EmployeeService` and the order to into `EmployeeService`.</span>

#### Dependency Management
- <span style="color:#537288">🧐🌞 What if we could let someone else do the client Class's work, but in configuration files and not code ?</span>
	- <span style="color:#537288">**🌞 Let the Spring give a hand to make it easier, in the way of Dependency Management**</span>
	- <span style="color:#9a7db0">**--> Let use the basis of Spring's Inversion of Control (IoC) in the form of Dependency Injection (DI)**</span>
		- <span style="color:#9a7db0">How Spring manages the dependencies with IoC and DI ?</span>
		- Spring's going to wire up the dependencies and and do some sort of configuration, but this will be done outside of the actual code of the application. 
			- --> By following the inversion of control principle, Spring enables developers to define the rules of engagement the dependencies (the relationships between components) and configurations externally, without embedding them directly in the code.
			- --> By using dependency injection to connect objects and manage dependencies.
				- **First, dependency injection is used to wire up (connect) the objects by providing the necessary dependencies to an object**. This is done by granting the object access to the required dependencies without the object needing to create or manage them itself.
				- **Once the object has been properly wired up with its dependencies, it is managed by Spring**. The Spring framework takes care of managing the object, including handling its dependencies and configurations.
				- **Finally, the object the bean, now managed by Spring with fully injected dependencies and is regarded as the bean, is returned to the calling client**. The client now can use the object with all its dependencies properly set up without having to worry about manual management of dependencies.
		- <span style="color:#9a7db0">--> You can easily execute a method that involves delegation without a null pointer exception because Spring has wired up the beans</span>.

- > [!INFO]- The power of Spring's Inversion of Control 
> - Spring's IoC operates as a huge factory that looks up dependencies of a target class that it is instantiating. -> How ?
> - It creates instances of the dependencies themselves and it identifies those dependencies using some thing called Reflection. 
> - In the target class, uses the setters or constructor to inject those dependencies into the target class based on the instruction of configuration (in the format of metadata/XML/... ). The rules of engagement between components of target class are not in the code, not in some master client class, just in configuration.
> <span style="color:#9a7db0">**--> Now Spring's IoC has the dependencies between objects but NOT in code, in configuration**</span>.

##### Dependency Management With BeanFactory
- Let see how the spring decide what to inject for target class which required dependency injection.
- --> We Get a Handle on the Container
	- Spring provides the `org.springframework.beans.factory.BeanFactory` interface to create and manage our beans. (Bean refer objects that are instantiated, assembled, and managed, fully wired up with dependencies by the Spring IoC container)
	- The <span style="color:#9a7db0">**BeanFacotry**</span> implementation is the actual container which instantiates configures, and manages a number of beans through its API: 
		- ![](https://i.imgur.com/u0nW6rz.png)
##### The ApplicationContext
- Interact with your Managed Beans
- **The ApplicationContext builds on top of the BeanFactory**
	- A sub interface of BeanFactory
	- Adds other functionality such as:
		- Easier integration with Spring's AOP
		- Message resource handling (for use in internationalization)
		- Event propagation
		- Declarative mechanisms to create the Application contexts.
		- Parent contexts
		- Application-layer specific contexts such as the `WebApplicationContext`
- **`BeanFactory` provides the configuration framework and basic functionality, and the `ApplicationContext` adds more enterprise-specific functionality**. 
