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




## Servlet Application
Spring Security integrates with the Servlet Container by using a standard `Servlet Filter` --> can work with any application runs in a Servlet Container
- Servlet Container
  > [!INFO] Servlet Container
   > [![](https://i.imgur.com/RBkQLnG.png)](https://www.baeldung.com/java-servlets-containers-intro)
### Started
- For supporting the Servlet Security, SprSe coordinate with Spring Boot to set the default configurations when you integrated for your servlet, so that it will have these following behaviors in runtime:
	![](https://i.imgur.com/HgLhA00.png)
- Let understand how SprBoot is coordinating with SprSe to achieve this by taking a look at <span style="color:#8d8d2a">Boot's security auto configuration</span> for example :
	```Java
		@EnableWebSecurity 1
		@Configuration
		public class DefaultSecurityConfig {
		    @Bean
		    @ConditionalOnMissingBean(UserDetailsService.class)
		    InMemoryUserDetailsManager inMemoryUserDetailsManager() { 2
		        String generatedPassword = // ...;
		        return new InMemoryUserDetailsManager(User.withUsername("user")
	                .password(generatedPassword).roles("ROLE_USER").build());
		    }
		    @Bean
		    @ConditionalOnMissingBean(AuthenticationEventPublisher.class)
		    DefaultAuthenticationEventPublisher  defaultAuthenticationEventPublisher(ApplicationEventPublisher delegate) { 3
	        return new DefaultAuthenticationEventPublisher(delegate);
		    }
		}
	```
	- 1 - `@EnableWebSecurity` --> help to <span style="color:#7030a0">publishes Spring Security's default Filter Chain as a @Bean</span>
	   > [!NOTE] @EnableWebSecurity
       > - SprBoot <span style="color:#d7771d">adds any filter published as a @Bean to the application's filter chain</span> ---> using `@EnableWebSecurity` in conjunction with <span style="color:#d7771d">Spring Boot automatically registers SprSe's filter chain for every request</span>
	- 2 - Publishes a `UserDetailsService` @Bean with the generated account by the system, this account has username default is `user`, role default is `ROLE_USER`, password is the which randomly generated by the system that is logged to the consoled 
    <span style="color:#555555">*(has the format like this : "the password is `8e557245-73e2-4286-969a-ff57fe326336`")*</span>
	- 3 - Publishes an  [`AuthenticationEventPublisher`](https://docs.spring.io/spring-security/reference/servlet/authentication/events.html) @Bean for <span style="color:#7030a0">publishing authentication events</span>
- So when use need implement Security
	- Building <span style="color:#d4a216">Rest API</span> - need <span style="color:#d4a216">Authenticate a JWT</span> || Other <span style="color:#d4a216">Bearer Token</span>
	- Building <span style="color:#d4a216">Web Application, API Gateway</span>, Or <span style="color:#d4a216">BFF</span> 
		- need to login using OAuth 2.0 or OIDC
		- need to login using SAML 2.0
		- need to login using CAS
	- Need to manage 
		- Users in LDAP || Active Director, with Spring Data || with JDBC
		- Password
	- Security <span style="color:#d4a216">Protocol</span> which your application use to communicate (For servlet-based app -> SprSe supports HTTP, Web Sockets)
	- For <span style="color:#d4a216">Authentication</span> + need <span style="color:#d4a216">authentication stateful || stateless</span>
	- For <span style="color:#d4a216">Authorization</span> 
	- <span style="color:#d4a216">Defense</span> - SprSe support defaults protections + additional protection you need
### Architecture
- Discusses SprSe's high-level architecture within Servlet based applications
#### A review of Servlet Filters
- The filters is really powerful in helping SprSe building a secure Servlet Application
	- Java Servlet Filter
	> [!INFO] [Reference Link](https://openplanning.net/10395/java-servlet-filter)
	>
	>![](https://i.imgur.com/Hagt6IX.png)
	>![](https://i.imgur.com/xYYQKQP.png)
	>![](https://i.imgur.com/LCr9Tep.png)
 <span style="color:#d7771d"> -> SprSe support Servlet security based on Servlet Filters</span>.
 ![](https://i.imgur.com/NglrX3O.png)
 > Figure FilterChain - shows the typical layering of the handlers for a single HTTP request
  ---
- Explain flow working
	 - Client sends a request to the application
	 - Container creates a `FilterChain` - which contains the <span style="color:#d7771d">`Filter` instances and Servlet</span> (indicated by the `DispatcherServlet` based on the path of the request `URI`) to process the coming `HttpServletRequest`
		>- Servlet is an instance of [`DispatcherServlet`](https://docs.spring.io/spring-framework/docs/6.1.0/reference/html/web.html#mvc-servlet) 
		>- Servlet is a class which responsible for accepting a request, processing it, and sending a response back -> handles a single `HttpServletRequest` and `HttpServletResponse`
	- `Filter` comes to the picture with its <span style="color:#d7771d">power comes from the `FilterChain`</span> that where `filter` is passed into it, by using this power we can
		- **Prevent downstream** `Filter` instances or the Servlet from being invoked. (The Filter in this case typically  writes the `HttpServletResponse`)
		- **Modify** the `HttpServletRequest` or `HttpServletResponse` used by the downstream `Filter` instances and the `Servlet`.
	- > [!Note] Note
	  > Since a filter in advance impacts only downstream Filter (For ex : Filter 2 can be impact by Filter 1, Filter 3 can be impact Filter 2, Filter 1 can disrupt results in Filter 3, but directly to Filter 2)
	  >> [!Attention]
	  >> The order in which each Filter is invoked is extremely important
- Usage FilterChain example
	``` Java
		public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) {
		// do something before the rest of the application
	    chain.doFilter(request, response); // invoke the rest of the application
	    // do something after the rest of the application
	}
	```
