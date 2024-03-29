---
{"dg-publish":true,"dg-permalink":"spring-security","permalink":"/spring-security/","noteIcon":"1"}
---


# SPRING SECURITY 6.2.0 TECH NOTES 


### 2.3 Authentication
   <span style="color:#d4a216">2 Parts</span>
- The Servlet Authentication Architecture - concentrate on abstract describing the architecture without much discussion on how it applies to concrete flows
- The Authentication Mechanisms - concentrate on concrete ways in which users can authenticate
#### 2.3.1 Part 1 - Discuss Servlet Authentication Architecture
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
#### 2.3.2 Part 2 - Authentication Architecture
- ![](https://i.imgur.com/aWbkAYo.png)
- ![](https://i.imgur.com/LSFK7AQ.png)
   [🍎 Source 👆](https://docs.spring.io/spring-security/reference/servlet/authentication/architecture.html#servlet-authentication-abstractprocessingfilter)
- ![](https://i.imgur.com/yDpbklX.png)
##### 2.3.2.1 SecurityContextHolder
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
##### 2.3.2.2 SecurityContext
- Obtained from the `SecurityContextHolder`
- Contains an `Authentication` object
##### 2.3.2.3 Authentication
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

##### 2.3.2.4 GrantedAuthority
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
##### 2.3.2.5 AuthenticationManager
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
##### 2.3.2.6 ProviderManager
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
##### 2.3.2.7 AuthenticationProvider
- Can inject mutiples `AuthenticationProviders` instances into ProviderManager
- Each `AuthenticationProviders` performs a specific type of authentication
- Ex: 
	- DaoAuthenticationProvider supports username/password-based authentication
	- `JwtAuthenticationProvider` supports authenticating a JWT token
- Another existing AuthenticationProviders which Spre Support [[Software Knowledges/Programming Language/Java/Java Framework/Java Spring Security/Spring Security 6.2.0#^Authentication-Providers\|#^Authentication-Providers]]
##### 2.3.2.8 Request Credentials with AuthenticationEntryPoint
- This is used to send an HTTP response that requests credentials from a client
- If the request is to a resource -> SprSe does not need to provide HTTP response that requests credentials from the client (since they are already included)
- <span style="color:#d4a216">Another case to show the useful of Request Credentials with `AuthenticationEntryPoint` is that when a client makes an unauthenticated request to a resource -> the are not authorized to access -> an implementation of `AuthenticationEntryPoint` is used to request back the credentials from the client by how -> it might perform a redirect to a login page and respond with an [WWW-Authenticate](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/basic.html#servlet-authentication-basic) header, or take other action</span>.
##### 2.3.2.9 AbstractAuthenticationProcessingFilter
- It is used as a base Filter for authenticating a user's credentials
- Before the credentials can be authenticated --> SprSe typically requests the CREDENTIALS by using `AuthenticationEntryPoint` (in the case request to the resource but not login)
- If the credentials of client exists -> the `AbstractAuthenticationProcessingFilter` can authenticate any authentication requests that are submitted to it
- [![](https://i.imgur.com/LSFK7AQ.png)](https://docs.spring.io/spring-security/reference/servlet/authentication/architecture.html#servlet-authentication-abstractprocessingfilter)

### 2.4 Authorization
<span style="color:#91819c">This section concentrates to configure your application's authorization rules</span>
- Consider attaching authorization rules to request URIs and methods
- Consider how Spring Security authorization works
- Describes the SprSe architecture that applies to authorization
- Authorities
	- Authentication has referred that all the implementations of Authentication Interface <span style="color:#d4a216"> store a list of `GrantedAuthority` objects</span> - represent the authorities (quyền hạn) that have been granted to the principal
	- 2.4.1 The `GrantedAuthority` objects -> <span style="color:#d4a216">inserted into the Authentication obj by the `AuthenticationManager`</span> & are later <span style="color:#d4a216">read by `AccessDecisionManager` instances when making authorization decisions</span>
	    ![](https://i.imgur.com/a1AVLqW.png)
	    ![](https://i.imgur.com/deoduir.png)
	  > [!Note] Method `String getAuthority();`
	  > This method is used by an `AuthorizationManager` instance to obtain a precise **String** **representation of the `GrantedAuthority`**-> this String - a `GrantedAuthority` (thằng đại diện cho thằng này là cái string được trả về từ `String getAuthority();`) can be "read" easily by most `AuthorizationManager` implementations
	  
	  > [!Caution] String is null
	  > - If a `GrantedAuthority` cannot be precisely represented as a String - be considered "complex" -> `getAuthority()`will be must return null.
	  > - The "complex" of `GrantedAuthority` would be an implementation that stores a List of operations and authority thresholds(for example, instead contains one Role, the implementation stores a list different roles) --> representing this complex GrantedAuthority as a String (this refer have to read the array) would be DIFFICULT --> As a result, the getAuthority() method should return null.
	  > ==> any AuthorizationManager want to support the specific GrantedAuthority (for ex: for role USER || ADMIN retrieve from the list which contain both USER, ADMIN) -> need to support the specific `GrantedAuthority` implementation (for example: implementing classes `OAuth2UserAuthority`, `OcidcUserAuthority`,..) to understand its "complex" contents
		- <span style="color:#d4a216">RESOVE HOW ?</span>
		   - SprSe includes one concrete `GrantedAuthority` implementation: `SimpleGrantedAuthority` --> this implementation lets any user-specified `String` be converted into a `GrantedAuthority` (sẽ kiểu đọc từ danh sách các role, lấy ra một role và convert sang Sring). All `AuthenticationProvider` instances included with the security architecture use `SimpleGrantedAuthority` to populate the `Authentication` object.
			- <span style="color:#555555">**Configuration:** </span> 
			 ``` Java
			  public class MyDatabaseUserDetailsService implements UserDetailsService {
			    UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
			       User user = userDao.findByUsername(username);
			       List<SimpleGrantedAuthority> grantedAuthorities = user.getAuthorities().map(authority -> new SimpleGrantedAuthority(authority)).collect(Collectors.toList()); // (1)
			       return new org.springframework.security.core.userdetails.User(user.getUsername(), user.getPassword(), grantedAuthorities); // (2)
			    }
			  }
			  ```
	- 2.4.2 <span style="color:#555555">The Invocation Handling - Handle the spring security exceptions</span>
		- <span style="color:#555555">**SprSe provides interceptors** (sự chen giữa/chen ngang) --> that control access to secure (such as method invocations or web request)</span> 
		- <span style="color:#555555">**A pre-invocation** decision on whether the invocation is allowed to proceed - this is made by `AuthorizationManager` instances</span>
		- <span style="color:#555555">**A post-invocation** decisions on whether a given value may be returned - this is made by `AuthorizationManager` instances</span>
	     <span style="color:#81ed0c">So, what is `AuthorizationManager` and how it make access control decision</span>
		---
		- The `AuthorizationManger` - what is ?
			- `AuthorizationManager` supersedes both `AccessDecisionManger` and `AccessDecisionVoter` - [link](#^AccessDecisionManager-and-AccessDecisionVoter)
			- > [!Caution]
		      > Applications that customize an `AccessDecsionManager` or `AccessDecisionVoter` need to change to use `AuthorizationManager` (because from Spr 6.0.0 the `AccessDecisionManager` has been deprecated) 
			- `AuthorizationManager`s **are called by SprSe's** 
				- <span style="color:#ff0000">request based authorization - Authorize `HttpServletRequests`</span>
				- <span style="color:#ff0000">method-based authorization - Method Security</span> 
				- <span style="color:#ff0000"> message-based authorization - WebSocket Security</span>
			- `AuthorizationManager` are responsible for making final access control decisions. 
			- The interface contains two methods
				![](https://i.imgur.com/uRDrasQ.png)
			- `check` method is passed all the relevant information it needs in order to make an authorization decision - information: authentication, secured object
				- the `check` method in implementations (các class triển khai `AuthorizationDecision`) expected return 
					- POSITIVE `AuthorizationDecision` access is GRANTED
					- NEGATIVE `AuthorizationDecision` access is DENIED
					- NULL `AuthorizationDecision` decision is ABSTAIN
					  <span style="color:#555555">(compare to `AccessDecisionVoter` which the implementations vote method return 3 kind of static field name: ACCESS_ABSTAIN, ACCESS_GRANTED, ACCESS_DENIED)</span> - [[Software Knowledges/Programming Language/Java/Java Framework/Java Spring Security/Spring Security 6.2.0#^AccessDecisionVoter-static-fields-named\|#^AccessDecisionVoter-static-fields-named]]
			- `verify` method call check, throw `AccessDeniedException` when check method return NEGATIVE `AuthorizationDecision`
		- Delegate-based `AuthorizationManager` Implementations - how does ?
			- <span style="color:#555555">To help `AuthorizationManager` to control access decision --> this interface has its own a composition(tổ hợp) </span>of `AuthorizationManager` implementations in the following picture here:  
			- ![](https://i.imgur.com/ybFnvs4.png)
			- ![](https://i.imgur.com/iUbDgaM.png)
			- SprSe ships with a delegating `AuthorizationManager` - collaborate with individual `AuthorizationManager`s
			- <span style="color:#d4a216">Match request</span> - `RequestMatcherDelegatingAuthorizationManager` - an implementation of `AuthorizationManager` interface --> match the request with the most appropriate delegate `AuthorizationManager` (delegates to a specific [`AuthorizationManager`](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/authorization/AuthorizationManager.html "interface in org.springframework.security.authorization") based on a [`RequestMatcher`](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/web/util/matcher/RequestMatcher.html "interface in org.springframework.security.web.util.matcher") evaluation)
			- <span style="color:#d4a216">Method Security</span> - can use 
				- `AuthorizationManagerBeforeMethodInterceptor` - A `MethodInterceptor` which uses a `AuthorizationManager` to determine if an `Authentication` may invoke the given `MethodInvocation` (the method that is assigned for specific authority - ex: a function only allow role admin)
				- `AuthorizationManagerAfterMethodInterceptor`- A MethodInterceptor which can determine if an Authentication has access to the result of an MethodInvocation using an AuthorizationManager
				  <span style="color:#555555">The mechanic that Method Security - these interceptors apply is the AOP -</span> [Aspect Oriented Programming with Spring](https://docs.spring.io/spring-framework/reference/core/aop.html)
				- 🍎 Reference: [Method Security](https://docs.spring.io/spring-security/reference/servlet/authorization/method-security.html)
			- AuthorityAuthorizationManager - [See](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/authorization/AuthorityAuthorizationManager.html)
				- <span style="color:#555555">An AuthorizationManager that determines if the current user is authorized by evaluating if the Authentication contains a specified authority.</span>
				- <span style="color:#555555">Return positive `AuthorizationDecision` should the `Authentication` contain any of the configured authorities (authorities which specified during the configuration by `@Secured({ "ROLE_VIEWER", "ROLE_EDITOR" })`, `@RolesAllowed("ROLE_VIEWER")` annotations for ex). It will return a negative `AuthorizationDecision` otherwise.</span>
			- AuthenticatedAuthorizationManager - [See](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/authorization/AuthenticatedAuthorizationManager.html)
				- <span style="color:#555555">An AuthorizationManager that determines if the current user is authenticated.</span>
				- <span style="color:#555555">Can be used to differentiate between anonymous, fully-authenticated and remember-me authenticated users.</span>
		- Custom Authorization Manager
			-  <span style="color:#555555">The purpose of Authentication Manager is given the approach to make decision access on authenticated user base on it's authorities(roles)</span>
			- 🍎 Reference: [Custom Authorization with Spring Boot](https://insource.io/blog/articles/custom-authorization-with-spring-boot.html)
		- Hierarchical Roles
			- requirement that a particular role in an application should automatically "include" other roles.
			- The use of a role-hierarchy allows you to configure which roles (or authorities) should include others.
			-  An extended version of Spring Security’s `RoleVoter`, `RoleHierarchyVoter`, is configured with a `RoleHierarchy`, from which it obtains all the "reachable authorities" which the user is assigned.
			- <span style="color:#91819c">Confiugration: Hierarchical Roles Configuration</span>
			- ![](https://i.imgur.com/ltEDPuX.png)
	    - > [!Info] `AccessDecisionManger` and `AccessDecisionVoter`
{ #AccessDecisionManager-and-AccessDecisionVoter}
	
	       > [!Info] `AccessDecisionManger`
           > - The `AccessDecisionManager` is called by the `AbstractSecurityInterceptor` 
	       > - <span style="color:#d4a216">Responsible for making final access control decisions</span>.
	       > - It is an interface, contains 3 methods:
	       >	```Java
	       >	void decide(Authentication authentication, Object securedObject, Collection<ConfigAttribute> attrs) throws AccessDeniedException;		
	       >	boolean supports(ConfigAttribute attribute);	
	       >	boolean supports(Class clazz);
	       >	```
	       > - The decide` method is passed all the relevant information it needs - (information are "authentication obj", the "secured object", the configuration attributes associated with the "secured object" being invoked) ---> to <span style="color:#d4a216">make an authorization decision</span>
	       > - `The supports(ConfigAttribute)` method is called by the `AbstractSecurityInterceptor` at startup time ---> <span style="color:#d4a216">determine if the `AccessDecsionManager` can process the passed `ConfigAttribute`</span>.
	       >	- `The suports(Class)` method - called by a security interceptor implementation to <span style="color:#d4a216">ensure the configured `AccessDecisionManager` supports the type of secure object </span>that the security interceptor presents
	       >	- <span style="color:#81ed0c">But this has been `Deprecated`, the replacement is `AuthorizationManager`</span>
	
           > [!Info] `AccessDecisionVoter` - Voting-Based `AccessDecisionManager` Implementations
	       > - While users can implement their own `AccessDecisionManager` to control all aspects (liên quan khái niệm AOP - [Aspect Oriented Programming with Spring](https://docs.spring.io/spring-framework/reference/core/aop.html) ) of authorization.
	       > - Spring Security includes several `AccessDecisionManager` implementation are based on voting.
	       >  ![](https://i.imgur.com/Up3oQgo.png)
	       >  <span style="color:#555555"> The picture is Voting Decision Manager - describes the relevant classes</span>
	       > - <span style="color:#91819c">By using this approach --> a series of `AccessDecisionVoter` implementations (like RoleVoter, AuthenticatedVoter, or your new CustomVoter,...) are polled on an authorization decision ---> then the `AccessDecisionManager` decides whether or not to throw an `AccessDeniedException` based on its assessment of the votes.</span>
	       >   ![](https://i.imgur.com/QIwNsvX.png)
	       > - <span style="color:#00b0f0">Note that</span>, the concrete/the voting implementations of `AccessDecisionVoter` interface have to return an int - with possible values being **reflected in the `AccessDecisionVoter` static fields named**: 
	       >     <span style="color:#91819c">ACCESS_ABSTAIN - A voting implementation returns this when if it has no opinion on an authorization decision</span>
	       >     <span style="color:#91819c">ACCESS_DENIED || ACCESS_GRANTED (1 of 2): A voting implementation returns either of them if it does have an opinion</span>
	       > - In the picture, we can see that `AccessDecisionManager` has three concrete - regard as `AccessDecisionVoter` - to give the decision of access denied for current authentication obj: 
	       >   ![](https://i.imgur.com/Ky5VzlP.png)
	       >   - The `ConsensusBased` implementation grants or denies access based on the consensus of non-abstain votes (phiếu trắng)
	       >   - The `AffirmativeBased` implementation grants access if one or more `ACCESS_GRANTED` votes were received
	       >   - The `UnanimousBased` provider expects unanimous `ACCESS_GRANTED` votes in order to grant access, ignoring abstains.
	       > - 🍎 Reference
	       >   [Voting-Based AccessDecisionManager Implementations](https://docs.spring.io/spring-security/reference/servlet/authorization/architecture.html#authz-voting-based)
	       >> [!Info] RoleVoter
	       >> - This treats configuration attributes as role names -> <span style="color:#d4a216">votes to grant access</span> if the user has been assigned that role
	       >> - It votes if any ConfigAttribute begins with the ROLE_ prefix
	       >> - It votes to grant access if there is `GrantedAuthority` 's representation return a String (from the `getAuthority()` method) exactly equal to one or more `ConfigAttributes` that that start with the ROLE_ prefix. If no -> vote to deny access, if `ConfigAttributes` begins with ROLE_ (not thing following) -> voter abstains. 
	       >>   ![](https://i.imgur.com/OwDw2Cf.png)
	       > - 🍎 Reference: Custom Voters 
	       > - [Custom AccessDecisionVoters in Spring Security](https://www.baeldung.com/spring-security-custom-voter)
	       > - [Configuring Spring Boot’s Method Level Security To Check Only The Required Permission](https://medium.com/@adammcg97/configuring-spring-boots-method-level-security-to-use-a-non-cached-source-to-check-for-7df7317127d2)
	       > - [Spring Security: Custom Access Decision Voter](https://blog.jdriven.com/2019/10/spring-security-custom-access-decision-voter/)
	       >> [!Caution]  
	       >> `AccessDecisionVoter` and `AccessDecisionManager` are not recommend to use, instead you should use `AuthorizationManager` above, this section is included for historical purpose
	<span style="color:#00b050">AND CONTIUNUE...</span>



