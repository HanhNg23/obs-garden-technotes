---
{"dg-publish":true,"permalink":"/Programming Languages/Java/Java Core/Variables/","title":"Language Basic Variables","noteIcon":"1","updated":"2024-05-04T21:01:28.431+07:00"}
---

<span style="color:#6a5858; font-size: 85%;">In Java, variables are used to store data values that can be accessed and manipulated by the program. They are defined by a data type, a name, and an optional initial value. This note covers the types of variables in Java, the rules and conventions for naming fields, and how variables are managed and manipulated by the program.</span>

## The variable  types
### Instance Variables - Non-static Fields
- Objects store their individual states in "non-static fields" == fields declared without the static keyword.
- Non-static fields - known as instance variables because their values are unique to each instance of a class (to each object).
	- *For example, the `currentSpeed` of bicycle-A object is independent from the `currentSpeed` of bicycle-B object.*
<div><img src="https://www.w3resource.com/w3r_images/java-class-image.png" alt="Image"> <p style="font-size: 70%;">Image from w3resource.com "Java Class, methods, instance variables"</p></div>


### Class Variables - Static Fields
- A class variable - field declared with the static modifier --> tells the compiler **"there is exactly one copy of this variable in existence, regardless of how many times the class has been initiated"** 
	- *For example, A field defining the number of gears for a particular kind of bicycle - regarded as class bicycle - could be marked as `static`. Since conceptually the same number of gears will apply to all instances of all instances of class bicycle.*
		- *The code `static int numGears = 6;` would create such a static field of class bicycle. --> bicyle A, B, C objects use the same numGears is 6.*
		- *Additionally, the keyword `final` could be added to indicate that the number of gears will never change.*
<div> <img src="https://javaconceptoftheday.com/wp-content/uploads/2016/07/ClassVariableVsInstanceVariable.png" alt="Image"> <p style="font-size: 70%;">Image from javaconceptoftheday.com "Class Variables And Instance Variables In Java
"</p></div>

###  Local Variables
- It is used in method block, a method will often store its temporary state in local variables.
- Local variables are only visible and accessed within to the function in which they are declared --> they are not accessible from the rest of the class.
<div> <img src="https://res.cloudinary.com/practicaldev/image/fetch/s--VaUTFAto--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://1.bp.blogspot.com/-_KKgYsgtkXY/XQIYE3IBwvI/AAAAAAAAIMU/TpDi88cbMqMgeCyIJ2H6JHllDPuZtsoagCLcBGAs/s1600/variables.png" alt="Image"> <p style="font-size: 70%;">Image from dev.to Hamid Mohamadi "Lesson 4: Variables In Java"</p></div>

###  Parameter
- Parameters - for example `public static void main(String[] args)`. Here, the `args` variable is the parameter to this method
- Parameters are always classified as "variables" not "fields"
- <div> <img src="https://mathbits.com/MathBits/Java/Methods/methodspic.jpg" alt="Image"> <p style="font-size: 70%;">Image from mathbits.com "What is Actually Passed to a Method ?"</p></div>


## Naming Conventions
- Variable names are case-sensitive. Allow an unlimited-length sequence of Unicode letters and digits, allow begin with a letter, the dollar sign "`$`", or the underscore character "`_`".
- In convention, name begin with a letter, should not "$" or "_", subsequent character can be letters, digits, dollar signs, or underscore characters.
- Variable name uses full words instead of cryptic abbreviations --> make it easier to read and understand
- If the name consists of only one word, spell that word in all lowercase letters.
	- *For example: cadence, speed, gear (should not a, b, c, d)*
- If the name consists of more than one word, capitalize the first letter of each subsequent word. 
	- *For example: gearRatio, currentGear*
- Constant Variable Name: capitalizing every letter and separating subsequent words with the underscore character "_". By convention, the underscore character is never used elsewhere.
	- *For example: static final int NUM_GEARS = 6*


## References
- [Java Variables](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/variables.html)