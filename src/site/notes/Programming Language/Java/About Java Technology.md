---
{"dg-publish":true,"permalink":"/programming-language/java/about-java-technology/"}
---

# About the Java Technology
- Java technology is both 
	- A programming language
	- A platform
## The Java Programming Language
- Java programming language is a high level language, there are many buzzwords to characterize this powerful programming language
	![](https://i.imgur.com/UTy6QiY.png)
- To understand why we use those buzzwords to describe Java programming --> let's go to this link [_The Java Language Environment_](http://www.oracle.com/technetwork/java/langenv-140151.html) - help to explain each buzzwords above
- The most feature of Java programming when it was first came out is the ability to <span style="color:#d4a216">"Write Once Run Anywhere - WORA</span>" --> Let's capture this concept:
	- In the Java programming language, all source code is first written in plaint text files ending with the <span style="color:#d4a216">".java"</span> extension
	- Those source files are then compiled by the <span style="color:#d4a216">"Javac compiler"</span> into <span style="color:#d4a216">".class" files</span> 
		- <span style="color:#555555">".class" files - a kind of file that contains the <span style="color:#d4a216">"Java bytecode"</span> - but does not contain code that is native to your computer processor (CPU) for executing directly which compares to the compiler of C language. This file is designed to be actually executed/read by a <span style="color:#d4a216">JVM(Java Virtual Machine)</span> - A <span style="color:#d4a216">machine-level language of JVM</span></span>
		- [The ClassFile Struture example](https://docs.fileformat.com/programming/class/#classfile-structure)
			- ![](https://i.imgur.com/YhQ62EH.png)
	- ![An overview of the software development process](https://docs.oracle.com/javase/tutorial/figures/getStarted/getStarted-compiler.gif)
	- The key to help Java Programming can run || capable of running on many different operating systems like Window, Solaris OS, Linux, or Mac OS is Java VM keyword - it is available on many different operating systems - be called as a <span style="color:#d4a216">platform-independent</span>. 
		- <span style="color:#555555">💡Note: JVM depends on the operating system - there are different Version JVM which is designed for specific operating system. For example, running Mac OS X you will have a different JVM than if running Windows or some other OS. ---> Reason to be come "Platform-independent" ---> Necessary to download the right JVM version which capable to your OS </span>
		- In additions, some virtual machines have features to give your application a performance boost, finding performance bottlenecks, recompiling to native code frequently used sections of code which performs at the runtime stage like [Java SE HotSpot at a Glance](http://www.oracle.com/technetwork/java/javase/tech/index-jsp-136373.html) for example. 
		- ![](https://i.imgur.com/ZUARtwb.png)

## The Java Platform
- <span style="color:#00b0f0">A platform is the hardware or software environment - a place in which a program runs</span>.
- Some of the most popular platforms like Microsoft Windows, Linux, Solaris OS and Mac OS.
- Also there are most platforms can be described as a combination of the operating system and underlying hardware.
- But come to the <span style="color:#d4a216">Java platform</span>, it differs from most other platforms in that it's <span style="color:#d4a216">a software-only platform</span> that runs on top of other hardware-based platforms.
- The Java platform has two components:
	- <span style="color:#d4a216">The _Java Virtual Machine_</span>
		- JVM(Java virtual machine) - is the base for the Java platform and is ported onto various hardware-based platforms (meaning: many kinds of JVM which is design for different OSs)
	- <span style="color:#d4a216">The _Java Application Programming Interface_ (API)</span>
		- The API is a large collection of ready-made software components that provide many useful capabilities.
		- It is group into the <span style="color:#d4a216">"Libraries of related classed and interfaces"</span> - known as "<span style="color:#d4a216">packages</span>" 

## Disadvantages of Java Technology
> [!INFO]
> As a platform-independent environment - the Java platform can be a bit slower than native code -- However, with advanced compiler and virtual machine technologies --> give the performance close to that of native code without threatening portability
 


## References
- [Think Java, Second Edition](https://books.trinket.io/thinkjava2/index.html)
- ```embed
title: "Java Best Practices"
image: "https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjjeodh-pPAFb7-3Gpa7ePnxhRWrsoNyFRvRBwQBNMvTrxcINBqFxQY3t9hQVyMIPEumG1mLzBDXY8_1UVzXB6lpDGNL4NNL8NNdQmePHrooNvazQVIIfBQO7Tawnetnc3wQCiqpivW8Lo/w1200-h630-p-k-no-nu/Java+Best+Practices.png"
description: "Java/J2EE Best Practices - 1. Java Standard Naming Conventions 2. Java Exception Handling Best Practices 3. Java Synchronization Best Practices"
url: "https://www.javaguides.net/p/java-best-practices.html"
```
