---
{"dg-publish":true,"dg-permalink":"spring-security","permalink":"/spring-security/","noteIcon":"1"}
---


# SPRING SECURITY 6.2.0 TECH NOTES 

---

## Feature Overview
As a framework, Spring Security provides support security in 3 aspects : Authentication - Authorization - Mechanism Protection Against Common Attacks 
### 1. Authentication
- What is authentication ? 
	Authentication - how we verify the identity of who is trying to access a particular resource.
   🍎 Reference: [Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)
---
*<span style="color:#9a7db0">In authentication, the most thing need to concern is the password</span>*
- Password Storage
	- Descript: password Storage indicate how to store the raw password securely by using encrypt password techniques in one-way transformation with the support of SprSe via Password Encoder Interface
		- History of encrypt password to store password securely [See](https://docs.spring.io/spring-security/reference/features/authentication/password-storage.html#authentication-password-storage-history)
		- Delegating Password Encoder with the support of SprSe
			- A mechanism for calling which encrypt password type will be invoke by choosing one of multiple of encrypt password type (BCrypt PasswordEncoder example)
			- Create instance of `DelegatingPasswordEncoder`
			- Create and config to custom `DelegatingPasswordEncoder`
			   ![](https://i.imgur.com/jaYS5W4.png)
	- Password Storage Format
		- How the format password save differ from raw text -  the encoded password format to avoid human readable ?
		- Example to show base on the password format Id - we can know to call which Password Encoder when `DelegatePasswordEncoder` instance
	      ![](https://i.imgur.com/uv1ODwk.png)
	- Password Encoding
		- Refer to `id` ForEncode - a sign to determines which PasswordEncoder is used for encoding passwords
	- Password Matching
		- Matching to identify the Password Encoder for encode password
	      ![](https://i.imgur.com/uv1ODwk.png)
	    <span style="color:#92d050">* By default, the result of invoking `matches(CharSequence, String)` with a password and an `id` that is not mapped (including a null id) results in an `IllegalArgumentException`. This behavior can be customized by using </span>
         `DelegatingPasswordEncoder.setDefaultPasswordEncoderForMatches(PasswordEncoder)`	 
		- Quick demo show password is really encoded	 
	      ![](https://i.imgur.com/5dZUUq3.png)
	- Password Storage Configuration 
		- Because DelegatingPasswordEncoder will call initial config - default, the PasswordEncoder will encode pass word using Bcrypt Password Encoder  like this
	      ![](https://i.imgur.com/MAjFdsE.png)
	      <span style="color:#c1ff80">*==> You want to config / custom default PasswordEncoder --->  custom default by 2 way*</span>
	   - Way 1 - custom DelegatingPasswordEncoder
		   ![](https://i.imgur.com/Cl4cU9g.png)
		- Way 2 - customize by exposing a **PasswordEncoder** as a Spring bean
		   <span style="color:#555555">Example:</span>
		   ![](https://i.imgur.com/5Jx89Kh.png)
		   ![](https://i.imgur.com/NLlRgw8.png)
	- Change Password Configuration
		- Extends process of [Forgot Password Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Forgot_Password_Cheat_Sheet.html)
> [!Tip] Summary 
><span style="color:#3adf97"> To implement password encoder in SprSe, do following : </span>
>  * 1st - Config which PasswordEncoder will in used for encoding password
>  * 2st - Config PasswordEncoder as Default DelegatingPasswordEncoder



