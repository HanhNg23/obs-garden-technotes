---
{"dg-publish":true,"permalink":"/Programming Languages/Java/Java Framework/Spring Framework/1 - Spring Introduction/","title":"Spring What is it ?","noteIcon":"1","updated":"2024-05-06T09:31:45.893+07:00"}
---


<span style="color:#6a5858; font-size: 85%;">Understanding the Spring framework explain the key concepts of an inversion of control independency injection and why Spring is so popular for programmers in developing enterprise applications.</span>

## What is Spring ?
- Spring is a framework, provides a programming safety net and configuration model for developing Java-based enterprise applications.
- Spring focuses on the "plumbing" of enterprise applications so that teams can focus on application-level business logic.
	- Spring focuses on creating the structure of your applications. There are the various beans that make up your application --> let you focus on the application logic itself and not worried about how classes communicate with each other because of its modular aspect. It covers both presentation layer components such as the Spring MVC framework.
- Spring is a lightweight framework - that addressed each tier in a Web application.
	- Presentation layer - Spring provides an MVC framework that is most similar to Struts but more powerful and easer to use.
	- Business layer - Spring enhances lightweight IoC (Inversion of Control) container helps to manage beans and provide AOP(Aspect orientation) support.
	- Persistence layer - Spring covers an abstraction of talking to the persistence layer of application by giving DAO templates which support for popular ORMs and JDBC.
- Spring is complimentary in that it manages implementations of Sub Systems through its own core distribution of Classes and configuration files to make a coders life easier. Means it provides supports and services for implementation such Rest Services, Persistence, Security.

## Why use Spring ?
 - Popular application development framework for enterprise Java
 - An open source Java platform
 - Organized in a modular fashion
 - Makes use of the existing technologies like several Persistence Frameworks, logging frameworks, Web Service Frameworks
 - Providing Testing framework makes it testable
 - Promotes decoupling and reusability
 - Allows developers to focus on business logic and less on pluming problems
 - Removes common code issues like leaking connections and more
 - Has built-in aspects such as transaction management
 - Built on design patterns - proven solutions that work


## Nuts and Bolts Of Spring 
- Spring is A Container
	- Creates Spring that manages objects and makes them available to your application
	- Uses a declarative XML structure or other types of metadata to define/configure components that "live" in the container
- Spring is A Framework
	- Provides an infrastructure of classes that make it easier to accomplish tasks
		- For example, In persistence layer, there are DAO templates that provide an abstraction away from the details to data access, remote calling.
	- Dependency Management - Creates specific instances of your Classes
		- Spring manages collaboration (dependencies) between Plain Old Java Objects (POJOs) 
		- Spring instantiates specific interface implementations
			- It means you don't need `"InterfaceType an object = new InterfaceImpl();"`. But uses Spring to provide the specific interface implementations of your objects