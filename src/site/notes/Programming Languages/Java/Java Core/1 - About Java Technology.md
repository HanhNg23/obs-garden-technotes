---
{"dg-publish":true,"permalink":"/Programming Languages/Java/Java Core/1 - About Java Technology/","title":"About The Java Technology","noteIcon":"1","updated":"2024-05-04T23:11:17.506+07:00"}
---

<span style="color:#6a5858; font-size: 85%;">
Java technology is both a programming language and a platform. In this post, I will cover the feature of Java Programming language, which is the capability "WORA - Write once, run anywhere," and some disadvantages of this technology.</span>

## The Java Programming Language
- Java programming language is a high level language, there are many buzzwords to characterize this powerful programming language
	- ![](https://i.imgur.com/UTy6QiY.png)
- To understand why we use those buzzwords to describe Java programming --> let's go to this link [_The Java Language Environment_](http://www.oracle.com/technetwork/java/langenv-140151.html) - help to explain each buzzwords above.
- The most feature of Java programming when it was first came out is the ability to <span style="color:#9a7db0">"Write Once Run Anywhere - WORA</span>" --> Let's capture this concept:
	- In the Java programming language, all source code is first written in plaint text files ending with the <span style="color:#9a7db0">".java"</span> extension
	- Those source files are then compiled by the <span style="color:#9a7db0">"Javac compiler"</span> into <span style="color:#9a7db0">".class"</span> files 
		- <span style="color:#9a7db0">".class"</span> files - a kind of file that contains the <span style="color:#9a7db0">"Java bytecode"</span> - but does not contain code that is native to your computer processor (CPU) for executing directly which compares to the compiler of C language. This file is designed to be actually executed/read by a JVM (Java Virtual Machine) in the machine-level language of JVM.		
		<div style="text-align: center; margin: 20px auto"> <img src="https://i.imgur.com/VyvDhkY.png" alt="Image" style="display: block; margin: 0 auto;"> <p style="position: relative; left: 50%; transform: translateX(-50%);font-size: 70%;"><a href="https://docs.fileformat.com/programming/class/#classfile-structure">The Class File Structure example</a> </p></div>
		<div style="margin: 20px auto"><img src="https://docs.oracle.com/javase/tutorial/figures/getStarted/getStarted-compiler.gif" alt="Image"> <p style="font-size: 70%;">An overview of the software development process</p></div>
	- The JVM is compatible with multiple operating systems, which means that the same ".class" files can be executed on various platforms such as Microsoft Windows, Solaris OS, Linux, or Mac OS.
		- ![](https://i.imgur.com/ZUARtwb.png)
	- There are some certain virtual machines, like [Java SE HotSpot at a Glance](http://www.oracle.com/technetwork/java/javase/tech/index-jsp-136373.html) , take extra measures during runtime to enhance the performance of your application. This involves actions like identifying performance limitations and frequently recompiling heavily used code sections into native code for improved efficiency.
		
    > [!NOTE] 💡Note
    > - The JVM is dependent on the operating system, with specific versions tailored for different OSs. 
    > 	- *For instance, using Mac OS X requires a distinct JVM compared to using Windows or another OS.* 
    > - For achieving "Platform-independent" functionality --> it is crucial to download the appropriate JVM version compatible with your OS. 

## The Java Platform
- **A platform is the hardware or software environment - a place in which a program runs**. Also there are most platforms can be described as a combination of the operating system and underlying hardware.
	- Some of the most popular platforms like Microsoft Windows, Linux, Solaris OS and Mac OS.
- **But come to the <span style="color:#9a7db0">Java platform</span>, it differs from most other platforms in that it's <span style="color:#9a7db0">a software-only platform that runs on top of other hardware-based platforms</span>**.
- The Java platform has two components:
	- <span style="color:#9a7db0">The _Java Virtual Machine_</span>
		- JVM (Java virtual machine) - is the base for the Java platform and is ported onto various hardware-based platforms (meaning: many kinds of JVM which is design for different OSs).
	- <span style="color:#9a7db0">The _Java Application Programming Interface_ (API)</span>
		- The API is a large collection of ready-made software components that provide many useful capabilities.
		- It is grouped into the "Libraries of related classed and interfaces" - known as "packages".
		- ![](https://i.imgur.com/wLcGZBE.png)

## Disadvantages of Java Technology
- As a platform-independent environment - the Java platform can be a bit slower than native code. However, with advanced compiler and virtual machine technologies --> give the performance close to that of native code without threatening portability.
 
## References
[About the Java Technology](https://docs.oracle.com/javase/tutorial/getStarted/intro/definition.html)