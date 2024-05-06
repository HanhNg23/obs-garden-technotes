---
{"dg-publish":true,"permalink":"/Programming Languages/Java/Java Framework/Spring Framework/2 - Dependency Injection/","title":"Spring Dependency Injection","noteIcon":"1","updated":"2024-05-06T15:45:59.806+07:00"}
---


<span style="color:#6a5858; font-size: 85%;">👓 In this post, i will delve into the concept of Inversion of Control (IoC) and its close connection to Dependency Injection (DI). We will also explore how the Spring framework supports the implementation of IoC through DI, revolutionizing the way software components are managed and dependencies are injected.👉 Let's uncover how these principles work together to enhance the modularity and flexibility of software development. Also, we'll delve into the core modules of the Spring Framework and their benefits for programmers.</span>

## Inversion of Control (IoC)
- IoC Definition
    > - <span style="color:#9a7db0">The basic concept of the **Inversion of Control pattern is that programmers don't need to create their objects**. *But instead, they need to Describe how they should be created through rules defined in e.g. factory classes and/or, other metadata, configuration mechanisms such as annotation*</span>.
	- --> Subsequently, your application code **does not directly connect your implementation components together** --> But instead **uses abstractions such as interfaces that represent them**. <span style="font-weight:bold; color:#d4a216">==> Spring provides the actual implementations</span>
		- <span style="color:#6a5858">*For example, Interface A and its implemented classed name B, C. We want to manage the implementation of instance of Interface A by using instance of Class B or C --> Spring supports manage the implementation by using the rules of instancing Interface A which has defined in Configuration file of Spring. --> The creation through a configuration will point Interface A to the actual implementation classes itself.*</span>
	> "*[Inversion of Control](https://www.baeldung.com/cs/ioc) is a principle in software engineering which transfers the control of objects or portions of a program to a container or framework*"
	> *From [baeldung.com Intro to Inversion of Control and Dependency Injection with Spring](https://jstobigdata.com/spring/inversion-of-control-and-dependency-injection-in-spring/#google_vignette)* 
- Mechanism to achieve IoC
	- We can achieve Inversion of Control through various mechanisms such as: Strategy design patter, Service Locator pattern. Factory pattern, and Dependency Injection.
	- <span style="color:#6a5858">In this post, we're going to look at Dependency Injection.</span>

## Dependency Injection
- <span style="color:#9a7db0">**Dependency Perception**</span>
	- <span style="color:#9a7db0">Dependency Injection is a design pattern that is employed to instantiate objects which rely on other objects for certain functionalities == objects that have delegation patterns to other objects.</span>
		- However, at compile time, the "class making the call to those objects above (regarded as the dependency classes of this calling class)" have no knowledge of the dependency class implementation. (For ex, The calling class want to call the instance of Interface A-dependency class, but does not know the specific implementation-class B, C). This is because the code of the Delegator Class (Class B,C ) is structured based on the abstract interface of the delegating class (refer to interface A act as delegating class).
	- <span style="color:#9a7db0">The need of Injection</span>
		- So, When interface A is required to be an instance of an implementation conforming to interface A itself, it might also include a property of type D, which is another interface. --> This property needs to be inserted into interface A, typically through a setter method or a constructor within the implementation class (class B, C) of the interface that defines this property **---> this process is called the injection**.
	- <span style="color:#d4a216">==> With Spring, the implementation of dependency injection can be achieved via setter and/or constructor injection techniques</span> 
> "*Dependency injection is a pattern we can use to implement IoC, where the control being inverted is setting an object’s dependencies. Connecting objects with other objects, or “injecting” objects into other objects, is done by an assembler rather than by the objects themselves.*"
> *From [baeldung.com Intro to Inversion of Control and Dependency Injection with Spring](https://jstobigdata.com/spring/inversion-of-control-and-dependency-injection-in-spring/#google_vignette)*

## The Spring IoC Container, work as a Spring Factory
> - <span style="color:#6a5858">An IoC container is a common characteristic of frameworks that implement IoC.</span>
> - <span style="color:#6a5858">In Spring framework, the interface `ApplicationContext` represents the IoC container --> Spring container is responsible for instantiating, configuring and assembling objects know as beans, as well as managing their life cycles.</span> 
- 🔎 We are going to explore Spring Factory to understand the bean/object creation and management.
- ![](https://i.imgur.com/GPlq5XJ.png)
	- <span style="color:#537288">**Configuration**</span>: Spring defines "beans" by utilizing a configuration using metadata which can be in XML format or annotations or even Java Factory Classes --> that's fed into the Spring factory.
	- <span style="color:#537288">**Application Context**</span>: Spring factory is called as an application context -> it reads the metadata and  generates an object representation of this metadata, which we refer to as bean definitions. Each bean definition has a primary identifier.
		- <span style="color:#878282">*The Spring framework offers different versions of the `ApplicationContext` interface for various types of applications. For standalone applications, it provides `AnnotationConfigApplicationContext`, `ClassPathXmlApplicationContext`, and `FileSystemXmlApplicationContext`. For web applications, it offers `WebApplicationContext`.*</span> 
	- <span style="color:#537288">**Object Representation of Configuration**</span>: From those bean definitions, instances of the implementation class associated with each bean definition are instantiated.
	- <span style="color:#537288">**Programmers API**</span>: When a request is made for bean - the primary identifier or alias (alternative bean ID) of the bean definition is used to retrieve the corresponding instance of the implementation class. 
	- <span style="color:#537288">**Request of Bean by Identifier**</span>: the request picks the instance up from the bean definition and says to the factory bean "Can your give me the bean whose ID is 123 (or something)" --> And the factory says "I've created the instance of class implementation for this bean definition. I'll go and get it for yo from the application context or cache, and I will return it to you". By the way, this class instance has already wired up all its dependencies.
- <span style="color:#d4a216">==> SPRING WIRES UP BEANS (Class instances) - The plumbing code is done for you.</span>
	- <span style="color:#878282">*The process of connecting and configuring beans is referred to as wiring up. This wiring process, which involves setting up the relationships between different components, is achieved through dependency injection, where the dependencies of an bean are injected into by the spring framework.*</span>
		- <span style="color:#878282">*--> Without Spring, you assemble everything yourself in code.*</span>
		- <span style="color:#878282">*--> With Spring, it assembles it for you and give you a ready to use object graph. So you can use these bean in your applications straight away. And if the class askes for delegates down to some of its properties or other classes, Spring has already created and injected them into the current class that you're dealing with.*</span>
		- ![](https://i.imgur.com/1BcwU8C.png)


## What does Spring Cover ?
- Identify the main modules of Spring and elaborate on the benefits Spring offers to programmers.
### Core Modules
- The Spring Framework comprises of many modules such as core, beans, context, expression language, AOP, Aspects, Instrumentation, JDBC, ORM, OXM, JMS, Transaction, Web, Servlet, Struts, etc.
- --> These modules are grouped into:
	- Test
	- Core Container
	- Aspect Oriented Programming (AOP)
	- Aspects
	- Instrumentation
	- Data Access/Integration 
	- Web (MVC/Remoting)
- ![](https://i.imgur.com/oKpJ9Oh.png)

### What do core modules do ?
- **Core and Beans**: these modules provide IoC and Dependency Injection features.
- **Context**: this module supports internationalization (118n), EJB, JMS.
- **Expression** Language: it is an extension to the EL defined in JSP, it provides support to setting and getting property values and even method invocation.
- **AOP, Aspects and instrumentation**: these modules aspect oriented programming implementation where you can use Advices, Pointcuts. The Aspects module provides support to integration with AspectJ.
- **Data Access/Integration**: these modules basically provide support to interact with the database.
- **Web**: this group provide support to create web applications.

