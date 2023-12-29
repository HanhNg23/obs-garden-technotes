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
  <span style="color:#d7771d">Figure Filter Chain - shows the typical layering of the handlers for a single HTTP request</span>
  ---
- Explain flow working
	 - Client sends a request to the application
	 - Container creates a `FilterChain` - which contains the <span style="color:#d7771d">`Filter` instances and Servlet</span> (indicated by the `DispatcherServlet` based on the path of the request `URI`) to process the coming `HttpServletRequest`
		- Servlet is an instance of [`DispatcherServlet`](https://docs.spring.io/spring-framework/docs/6.1.0/reference/html/web.html#mvc-servlet) 
		- Servlet is a class which responsible for accepting a request, processing it, and sending a response back -> handles a single `HttpServletRequest` and `HttpServletResponse`
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

### Authentication
   <span style="color:#d4a216">2 Parts</span>
- The Servlet Authentication Architecture - concentrate on abstract describing the architecture without much discussion on how it applies to concrete flows
- The Authentication Mechanisms - concentrate on concrete ways in which users can authenticate
#### Part 1 - Discuss Servlet Authentication Architecture
   - This is the expands on Architecture Section Above -  [Servlet Security: The Big Picture](https://docs.spring.io/spring-security/reference/servlet/architecture.html#servlet-architecture), describe the main architectural components of SprSe's used in Servlet Authentication, include following here :
	   -  <span style="color:#555555">SecurityContextHolder</span>
	   -  <span style="color:#555555">SecurityContextHolder</span>
	   -  <span style="color:#555555">Authentication (Obj)</span>
	   -  <span style="color:#555555">GrantedAuthority</span>
	   -  <span style="color:#555555">AuthenticationManager</span>
	   -  <span style="color:#555555">ProvideManager</span>
	   -  <span style="color:#555555">AuthenticationProvider</span>
	   -  <span style="color:#555555">Request Credentials with AuthenticationEntryPoint</span>
	   -  <span style="color:#555555">AbstractAuthenticationProcessingFilter</span>
#### Part 2 - Authentication Architecture
- ![](https://i.imgur.com/aWbkAYo.png)
- ![](https://i.imgur.com/LSFK7AQ.png)
   [🍎 Source 👆](https://docs.spring.io/spring-security/reference/servlet/authentication/architecture.html#servlet-authentication-abstractprocessingfilter)
- ![](https://i.imgur.com/yDpbklX.png)
##### SecurityContextHolder
- `SecurityContextHolder` - the heart of Spring Security's authentication model 
- <span style="color:#d4a216">It contains the `SecurityContext`</span>
- This is the place that SprSe stores the details of who is authenticated
- But do not care if `SecurityContextHolder` is populated
- *If contains a value --> it is used as the currently authenticated user*
- <span style="color:#81ed0c">For example, setting directly the indicated authenticated user in `SecurityContextHolder`</span>
	```Java
	SecurityContext context = SecurityContextHolder.createEmptyContext(); 1
	Authentication authentication =
	    new TestingAuthenticationToken("username", "password", "ROLE_USER"); 2
	context.setAuthentication(authentication);
	
	SecurityContextHolder.setContext(context); 3
	```
	1. <span style="color:#d4a216">Create empty `SecurityContext`</span> (Should create a new `SecurityContext` instance instead of using *SecurityContextHolder.getContext().setAuthentication(authentication)* --> avoid race conditions accross multiple threads)
	2. <span style="color:#d4a216">Create a new Authentication object</span> - SprSe <span style="color:#0070c0">does note care what type of Authentication</span> implementation is set on the `SecurityContext` - in our example, we use Authentication the type is `TestingAuthenticationToken` (the other popular type is UsernamePasswordAuthenticationToken(userDetails, password, authorities)
	3. <span style="color:#d4a216">Set the `SecurityContext` object on `SecurityContextHolder`</span> --> <span style="color:#3adf97">SprSe will use this information for Authorization</span>
- <span style="color:#d4a216">How to access `Currently Authenticated User`</span>
	```Java
		SecurityContext context = SecurityContextHolder.getContext();
		Authentication authentication = context.getAuthentication();
		String username = authentication.getName();
		Object principal = authentication.getPrincipal();
		Collection<? extends GrantedAuthority> authorities = authentication.getAuthorities();	
	```
- > [!NOTE] Clear the current `SecurityContext` instance after request processed
  > Imagine that you do not want save the current authenticated user in the `SecuritContext` obj after the process of request done for saving memory
  > -> Note that, `SecurityContextHolder` uses a `ThreadLocal` to store these details --> So the `SecurityContext` is always available to methods in the same thread (even if the `SecuirtyContext` is not explicitly passed around as an argument to those method - tức dù ko có method nào nhận SecurityContext làm tham số truyền vào thì tại thread hiện tại đang dùng vần còn lưu `SecurityContext`)
  > --> Using a `ThreadLocal` in this way is quite safe if you take care to clear the thread after the present principal's request is processed. But with <span style="color:#81ed0c">SprSe's `FilterChainProxy` ensures that the `SecurityContext` is always cleared</span>.
- > [!CAUTION]
  >  There is an issue is here that some applications are not entirely suitable for using a `ThreadLocal` - because of the specific way they work with threads -- for ex, Swing client might want all threads in a JVM to use the same security context.
  > <span style="color:#d4a216">Resolve --> You can configure `SecurityContextHolder` with a strategy on startup to specify how you would like the context to be stored</span>.
  > For example, config: 
  > 	- `SecurityContextHolder.MODE_GLOBAL` strategy
  >     - `SecurityContextHolder.MODE_INHERITABLETHREADLOCAL`
  > 🍎 Take a look at the JavaDoc for [`SecurityContextHolder`](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/core/context/SecurityContextHolder.html) to learn more.> 	
- > [!INFO] ThreadLocal
  > It is a mechanism to manage the multi thread and each its value
  > For example, if you want to store the value for the current thread is in used -> tell the thread local to "map the current thread with my value please !"--> The power of the Thread local is that avoid the current thread retrieving value from another thread, it just allows the current thread can set and get the values its own.
  > Reference :
  > 	- 🍎 [Cách xây dựng ThreadLocal trong Java](https://topdev.vn/blog/cach-xay-dung-threadlocal-trong-java/)
  > 	- 🍎 [An Introduction to ThreadLocal in Java](https://www.baeldung.com/java-threadlocal)
  > 	- 🍎 [Java ThreadLocal Example](https://www.digitalocean.com/community/tutorials/java-threadlocal-example)
  > 	- 🍎 [Tìm hiểu về ThreadLocal trong Java](https://viblo.asia/p/tim-hieu-ve-threadlocal-trong-java-Qbq5QaLE5D8)
##### SecurityContext
- Obtained from the `SecurityContextHolder`
- Contains an `Authentication` object
##### Authentication
- It is an interface - serves 2 main purposes within SprSe:
	- <span style="color:#d4a216">An input to `AuthenticationManager`</span> to provide the credentials which the user has provided to authenticate
	- <span style="color:#d4a216">Represent the currently authenticated user</span> --> So you can obtain the current Authentication from the `SecurityContext`
- > [!Info] Authentication contains :
  > - Principal: Identifies the user - *<span style="color:#d4a216">when authenticating with a username/password - this is often an instance of `UserDetails`</span>*
   > - Credentials: Often a password - In many cases, this is cleared after the user is authenticated to avoid memory leak 
   > - Authorities: The `GrantedAuthority` instances - high-level permissions the user is granted. 2 examples are ROLES and SCOPES
- > [!Info]  Implementing Classes derived from Authentication Interface 
> -  Authentication là một interface với mục tiêu chính là cho phép "truy cập được dưới dạng get, set" những thông tin cần thiết của người dùng hiện đang cần xác thực bao gồm các thông tin cho Principal(là username, phone..., object user blala), cho Credentials (là password), cho Authorities(các role, scope đồ đầy). 
  >	![](https://i.imgur.com/BxogUVV.png)
> - Vì nó là một interface chỉ cung cấp các phương thức truy cập thuộc tính, vậy thông tin về các thuộc tính gốc được để ở đâu -> để một trong các class triển khai từ bạn Authentication interface này. 
> -  Và trong SprSe cung cấp nhiều kiểu lưu thông tin Authentication qua các class triển khai sau đây : 
  >	![](https://i.imgur.com/WqpBYQg.png)
  >	
  > 🍎 Reference : [Interface Authentication](https://docs.spring.io/spring-security/site/docs/4.0.x/apidocs/org/springframework/security/core/Authentication.html)
  >
{ #Authentication-Implementing-Classes}

##### GrantedAuthority
- Có thể hiểu là label gán nhãn quyền hạn của người đăng nhập đối với app
- Obtain `GrantedAuthority` instances from the `Authentication.getAuthorities()` method - this method provides a Collection of `GrantedAuthority` objects.
	```Java
		SecurityContext context = SecurityContextHolder.getContext();
		Authentication authentication = context.getAuthentication();
		String username = authentication.getName();
		Object principal = authentication.getPrincipal();
		Collection<? extends GrantedAuthority> authorities = authentication.getAuthorities();	
	```
- -> A GrantedAuthority is an authority that is granted to the principal. Such authorities are usually "roles" (ROLE_USER, ROLE_ADMIN, ROLE_HR_SUPERVISOR) -> <span style="color:#81ed0c">these roles are later configured for **web authorization**, **method authorization**, **domain object authorization**</span>
- <span style="color:#81ed0c">For example, method authorization, there is a method that only allow access for some kind of roles</span> 
```Java
@Secured("ROLE_VIEWER") public String getUsername() { SecurityContext securityContext = SecurityContextHolder.getContext(); return securityContext.getAuthentication().getName(); }

@Secured({ "ROLE_VIEWER", "ROLE_EDITOR" }) public boolean isValidUsername(String username) { return userRoleRepository.isValidUsername(username); }

-----

@RolesAllowed("ROLE_VIEWER") public String getUsername2() { //... } 

@RolesAllowed({ "ROLE_VIEWER", "ROLE_EDITOR" }) public boolean isValidUsername2(String username) { //... }
```
- 🍎 Learn more: [Introduction to Spring Method Security](https://www.baeldung.com/spring-security-method-security)
- When using username/password based authentication then  `GrantedAuthority` instances are usually loaded by the [`UserDetailsService`](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/user-details-service.html#servlet-authentication-userdetailsservice)
- <span style="color:#81ed0c">Usually, the GrantedAuthority Objects are application-wide permissions - they are not specific to a given domain object</span>
##### AuthenticationManager
- Is the API - defines how SprSe's Filters perform authentication process to build `Authentication` obj - this means that the  `AuthenticationManager` help to specify which Authentication Mechanism is used for authentication
  - > [!NOTE]
   > Because there are various Authentication mechanisms or can call as Authentication Provider for implementation to authenticate the obj `Authentication`, for ex: 
   > ![](https://i.imgur.com/0K05mXr.png)
   > *<span style="font-style:italic; color:#d4a216">You can also create your own custom Authentication Provider</span>*
   > <span style="color:#7030a0">->  So these type will be manage by `AuthenticationManager` ---> we can tell `AuthenticationManager` which Authentication type you want to implement for authentication 😎 </span>
   > 
   > 🍎 Reference : [Spring Security – Authentication Providers](https://www.geeksforgeeks.org/spring-security-authentication-providers/) 
   >
{ #Authentication-Providers}

- The `Authentication` obj that is returned after the process of authentication done (do not care the user valid or invalid) is then set on the [SecurityContextHolder](https://docs.spring.io/spring-security/reference/servlet/authentication/architecture.html#servlet-authentication-securitycontextholder) by the controller that invoked the `AuthenticationManager`
- <span style="color:#d4a216">Because the `AuthenticationManager` is an interface -> the implementation of it could be anything. The most common implementation is `ProviderManager`</span>
##### ProviderManager
- `ProviderManager` - the most commonly used <span style="color:#d4a216">implementation of</span> `AuthenticationManager`
- <span style="color:#d4a216">`ProviderManager` delegates to a List of `AuthenticationProvider` instances</span>
- ![](https://docs.spring.io/spring-security/reference/_images/servlet/authentication/architecture/providermanager.png)
- > [!Info] About `AuthenticationProvider`
   > - Each `AuthenticationProvider` has an <span style="color:#d4a216">opportunity to indicate</span> that authentication should be successfull || fail || indicate it cannot make a decision -> then allow a downstream  `AuthenticationProvider` to decide
	> - If none of the configured `AuthenticationProvider` instances can authenticate -> <span style="color:#d4a216">authentication fails with exception</span> `ProviderNotFoundException` (mean - `AuthenticationException` indicates the `ProviderManager` was not configured to support the [[Software Knowledges/Programming Language/Java/Java Framework/Java Spring Security/Spring Security 6.2.0#^Authentication-Implementing-Classes\|type of Authentication]] that was passed into it)
	> - Each `AuthenticationProvider` knows how to perform a specific type of authentication
    > For ex, one `AuthenticationProvider` might be able use to validate a username/password (like `DaoAuthenticationProvider`) but another might be able to authenticate a SAML assertion (like )
- `ProviderManager` also allows configuring an optional parent `AuthenticationManager`
	- The parent can be any type of `AuthenticationManager`, but it is often an instance of `ProviderManager`
	 ![](https://i.imgur.com/E4vaBOj.png)
	-  Multiple `ProviderManager` instances might share the same parent `AuthenticationManager` - this is somewhat common in scenarios where there are <span style="color:#d4a216">multiple `SecurityFilterChain` instances have some authentication in common</span> but can also <span style="color:#d4a216">different authetication mechanisms</span> (the different ProviderManager instances)
- Default, <span style="color:#81ed0c">`ProviderManager` tries to clear any sensitive credentials information from the Authentication obj which is returned by a successful authentication request</span> -> avoid being retained longer than necessary in the `HttpSession` --> But can cause issues when use a cache of user objects (for ex to improve performance in a stateless application)
##### AuthenticationProvider
- Can inject mutiples `AuthenticationProviders` instances into ProviderManager
- Each `AuthenticationProviders` performs a specific type of authentication
- Ex: 
	- DaoAuthenticationProvider supports username/password-based authentication
	- `JwtAuthenticationProvider` supports authenticating a JWT token
- Another existing AuthenticationProviders which Spre Support [[Software Knowledges/Programming Language/Java/Java Framework/Java Spring Security/Spring Security 6.2.0#^Authentication-Providers\|#^Authentication-Providers]]
##### Request Credentials with AuthenticationEntryPoint
- This is used to send an HTTP response that requests credentials from a client
- If the request is to a resource -> SprSe does not need to provide HTTP response that requests credentials from the client (since they are already included)
- <span style="color:#d4a216">Another case to show the useful of Request Credentials with `AuthenticationEntryPoint` is that when a client makes an unauthenticated request to a resource -> the are not authorized to access -> an implementation of `AuthenticationEntryPoint` is used to request back the credentials from the client by how -> it might perform a redirect to a login page and respond with an [WWW-Authenticate](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/basic.html#servlet-authentication-basic) header, or take other action</span>.
##### AbstractAuthenticationProcessingFilter
- It is used as a base Filter for authenticating a user's credentials
- Before the credentials can be authenticated --> SprSe typically requests the CREDENTIALS by using `AuthenticationEntryPoint` (in the case request to the resource but not login)
- If the credentials of client exists -> the `AbstractAuthenticationProcessingFilter` can authenticate any authentication requests that are submitted to it
- [![](https://i.imgur.com/LSFK7AQ.png)](https://docs.spring.io/spring-security/reference/servlet/authentication/architecture.html#servlet-authentication-abstractprocessingfilter)

### Authorization
<span style="color:#91819c">This section concentrates to configure your application's authorization rules</span>
- Consider attaching authorization rules to request URIs and methods
- Consider how Spring Security authorization works
- Describes the SprSe architecture that applies to authorization
- Authorities
	- Authentication has referred that all the implementations of Authentication Interface <span style="color:#d4a216"> store a list of `GrantedAuthority` objects</span> - represent the authorities (quyền hạn) that have been granted to the principal
	- The `GrantedAuthority` objects -> <span style="color:#d4a216">inserted into the Authentication obj by the `AuthenticationManager`</span> & are later <span style="color:#d4a216">read by `AccessDecisionManager` instances when making authorization decisions</span>
		 ![](https://i.imgur.com/a1AVLqW.png)
		 ![](https://i.imgur.com/deoduir.png)
	  > [!Note] Method `String getAuthority();`
	  > This method is used by an `AuthorizationManager` instance to obtain a precise **String** **representation of the `GrantedAuthority`**-> this String - a `GrantedAuthority` (thằng đại diện cho thằng này là cái string được trả về từ `String getAuthority();`) can be "read" easily by most `AuthorizationManager` implementations
	  > [!Caution] String is null
	  > If a `GrantedAuthority` cannot be precisely represented as a String - be considered "complex" -> `getAuthority()`will be must return null.
	  > the "complex" of `GrantedAuthority` would be an implementation that stores a List of operations and authority thresholds(for example, instead contains one Role, the implementation stores a list different roles) --> representing this complex GrantedAuthority as a String (this refer have to read the array) would be DIFFICULT --> As a result, the getAuthority() method should return null.
	  > ==> any AuthorizationManager want to support the specific GrantedAuthority (for ex: for role USER || ADMIN retrieve from the list which contain both USER, ADMIN) -> need to support the specific `GrantedAuthority` implementation (for example: implementing classes `OAuth2UserAuthority`, `OcidcUserAuthority`,..) to understand its "complex" contents.
	  > 	
	  > <span style="color:#d4a216">RESOVE HOW ?</span>
	   > SprSe includes one concrete `GrantedAuthority` implementation: `SimpleGrantedAuthority` --> this implementation lets any user-specified `String` be converted into a `GrantedAuthority` (sẽ kiểu đọc từ danh sách các role, lấy ra một role và convert sang Sring). All `AuthenticationProvider` instances included with the security architecture use `SimpleGrantedAuthority` to populate the `Authentication` object
	     **<span style="color:#555555">Configuration:** </span> 
		 ``` Java
		  public class MyDatabaseUserDetailsService implements UserDetailsService {
		    UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
		       User user = userDao.findByUsername(username);
		       List<SimpleGrantedAuthority> grantedAuthorities = user.getAuthorities().map(authority -> new SimpleGrantedAuthority(authority)).collect(Collectors.toList()); // (1)
		       return new org.springframework.security.core.userdetails.User(user.getUsername(), user.getPassword(), grantedAuthorities); // (2)
		    }
		  }
		  ```