---
{"dg-publish":true,"dg-permalink":"spring-security","permalink":"/spring-security/","noteIcon":"1"}
---


# SPRING SECURITY 6.2.0 TECH NOTES 

---

## Feature
![](https://i.imgur.com/isjlcEr.png)
As a framework, Spring Security will support security in 3 aspects : Authentication : Authorization : Mechanism Protection Against Common attacks 
### 1. Authentication
	
- <What is authentication ? .... >
Reference: [Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)
The most thing need to concern is the password
- Password Storage
	- Descript: password Storage indicate how to store the raw password securely by using encrypt password techniques in one-way transformation with the support of SprSe via PasswordEncoder Interface
		- History of encrypt password to store password securely
		- Delegating Password Encoder with the support of SprSe
			* A mechanism for calling which encrypt password type will be invoke by choosing one of multiple of encrypt password type (BCrypt PasswordEncoder example)
			* Create instance of DelegatingPasswordEncoder
			* Create and config to custom DelegatingPasswordEncoder
			![](https://i.imgur.com/jaYS5W4.png)

- Password Storage Format
	* How the format password save differ from raw text -  the encoded password format to avoid human readable ?
	* Example to show base on the password format Id - we can know to call which Password Encoder when DelegatePasswordEncoder instance
	  ![](https://i.imgur.com/uv1ODwk.png)

- Password Encoding
	* Refer to idForEncode - a sign to determines which PasswordEncoder is used for encoding passwords

- Password Matching
	* Matching to identify the Password Encoder for encode password
	 ![](https://i.imgur.com/uv1ODwk.png)
	 <span style="color:#92d050">* By default, the result of invoking `matches(CharSequence, String)` with a password and an `id` that is not mapped (including a null id) results in an `IllegalArgumentException`. This behavior can be customized by using </span>
	  `DelegatingPasswordEncoder.setDefaultPasswordEncoderForMatches(PasswordEncoder)`	 

- Quick demo show password is really encoded	 
	![](https://i.imgur.com/5dZUUq3.png)
	
- Password Storage Configuration 
	- Because DelegatingPasswordEncoder will call initial config - default, the PasswordEncoder will encode pass word using Bcrypt Password Encoder  like this
	![](https://i.imgur.com/MAjFdsE.png)
	<span style="color:#c1ff80">	*==> You want to config / custom default PasswordEncoder --->  custom default by 2 way*</span>
   * Way 1 - custom DelegatingPasswordEncoder
		![](https://i.imgur.com/Cl4cU9g.png)
	* Way 2 - customize by exposing a **PasswordEncoder** as a Spring bean
		example :
		![](https://i.imgur.com/5Jx89Kh.png)
		![](https://i.imgur.com/NLlRgw8.png)

- Change Password Configuration
	- Extends process of [Forgot Password Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Forgot_Password_Cheat_Sheet.html)

> [!Tip] Summary 
><span style="color:#3adf97"> To implement **Spring Security Authentication**, do following : </span>
>  * 1st - Config which PasswordEncoder will in used for encoding password
>  * 2st - Config PasswordEncoder as Default DelegatingPasswordEncoder

----

### 2. Authorization

- What it is ? 
	* Indicate which doors you are allow to access 
	* Spring Security provides [defense in depth](https://en.wikipedia.org/wiki/Defense_in_depth_(computing)) by allowing for request based authorization and method based authorization.


### 3. Protection Against Exploits
#### CSRF
![](https://i.imgur.com/r0uwGSC.png)
-  What it is ?
	- An attack that the **evil will embedded the valid data** but **send the request to the server via the link embedded in the button and entices** the user to click the button to send the request which have already authenticated previously
			![](https://qph.cf2.quoracdn.net/main-qimg-128d7b63046fc10e74bf6346e7da0ed6)
			![](https://media.geeksforgeeks.org/wp-content/uploads/20220330113139/csrf.jpg)
- Protecting Against CSRF
	* Safe Methods Must be Read-only
		- ensure that ["safe" HTTP methods are read-only](https://tools.ietf.org/html/rfc7231#section-4.2.1) (? how )
		- Consider Implements the CSRF for specific perspective of web app
			- Logging In
			- Logging Out
			- CSRF and Session Timeouts
			- Multipart (file upload)
			- How to include CSRF Token
			- Place CSRF Token in the Body of the request
			- Include CSRF Token in URL
	* <span style="color:#4ee4b7">Synchronizer Token Pattern</span>
		- When an HTTP request is submitted -> server must look up the expected CSRF token -> compare against the actual CSRF token in HTTP request
		- The token must be store under the way as a part of the HTTP request || in the header --> so that the CSRF will not be automatically included by the browser 
		- To achieve, we can <span style="color:#2dd267">get the CSRF token in an HTTP parameter OR an HTTP header</span> --> protect against CSRF attacks
			(by this way, if hacker requires the actual CSRF token in a cookie --> not work because cookies are automatically included in the HTTP request by the browser but CSRF is stored in away avoid sending automatically with the request )
			![](https://i.imgur.com/EH1kK8P.png)
	* SameSite Attribute
		- Specify SameSite Attribute on cookies
		- A server can specify the SameSite attribute when setting a cookie --> to indicate that **the cookie should not be sent if the request is coming from external sites.
- <span style="color:#ffcd1a">Implement CSRF Protection In SprSe</span>: [Cross Site Request Forgery (CSRF)](https://docs.spring.io/spring-security/reference/servlet/exploits/csrf.html)
#### HTTP Header
 - What it is ?
	 - Manipulate using [HTTP response headers](https://owasp.org/www-project-secure-headers/#div-headers) to increase the security of web applications. Once set, these HTTP response headers can restrict modern browsers from running into easily preventable vulnerabilities.
	 - The Spring Security support various HTTP response headers, base on its idea of security of using headers, you can take benefits SrpSe in supporting headers and you can config, custom headers
- > [!TIP]
 > <span style="color:#4ee4b7">Let explore how Spring Security Support HTTP response headers ?</span>
- Default Spring Security Headers - SprSe provides a secure default set of security related HTTP response headers, the defaults include the following headers:
```
	Cache-Control: no-cache, no-store, max-age=0, must-revalidatePragma: no-cache
	Expires: 0
	X-Content-Type-Options: nosniff
	Strict-Transport-Security: max-age=31536000 ; includeSubDomains
	X-Frame-Options: DENY
	X-XSS-Protection: 0
```
> *--> If defaults do not meet your needs -> let remove, modify, add headers from these defaults.*
- <span style="color:#4ee4b7">Let explore additional details on each of above headers</span>
	- <span style="color:#4ee4b7">Cache Control</span>
		- *The main purpose of header in helping security is what ?*
			- Dictates browser caching behavior
		- *The case context you should need this header ?*
			- When someone visits a website, the browser will save certain resources: images, website data in a store called the "CACHED" -> When user revisits, the website, cached-control sets the rule which determine whether that user will have those resources loaded from their local cache OR whether the browser have to send a request to the server for fresh resources.
	    - *Why it is a matter ?*
			- Base on the working of cached control --> we need rule the cached control because "If a user authenticates to view sensitive information and then logs out, we do not want a malicious user to be able to click the back button to view the sensitive information from the cached"
	- <span style="color:#4ee4b7">Content Type Options</span>
		- *The main purpose of header in helping security is what ?*
	 		- used by server to indicate to the browsers that the MIME types advertised in the Content-Type headers -> browser should be followed and not guessed -> block browsers 'MIME type sniffing, which can transform non-executable MIME types into executable MIME types'
	 	- *The case context you should need this header ?*
	 		- Browser usually try to guess the content type of a request by using "Content Sniffing" to improve the user experience by guessing the content type on resources that had not specified the content type.	
	 	- *Why it is a matter ?*
	 		- Problem with content sniffing --> allowed malicious users to use polyglots (a file that is valid as multiple content types) to perform XSS attacks.	
	- <span style="color:#4ee4b7">HTTP Strict Transport Security (HSTS)</span>
		- *The main purpose of header in helping security is what ?*
			- add the url `mybank.example.com` (ex) with HTTP Strict Transport Security (HSTS) --> a browser can know ahead of time that any request to `HTTP` `mybank.example.com` should be interpreted as `HTTPs` `https://mybank.example.com` --> REDUCEs the possibility of a Man-in-the-Middle attack.
		- *The case context you should need this header ?*
			- For example, between 2 url of bank's website :
			`HTTP` - `mybank.example.com` vs `HTTPs` `https://mybank.example.com`  
			-> if omit the https protocol --> potentially vulnerable to [Man-in-the-Middle attacks](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) - a cyberattack where the attacker secretly relays and possibly alters the between two parties who believe that they are directly communicating with each other, as the attacker has inserted themselves between the two parties
			![](https://i.imgur.com/xaw0plU.png)
		- *Why it is a matter ?*
			- Base on the case context above, when you ignore https link access -> a malicious user could intercept the initial HTTP request like  `mybank.example.com` and manipulate the response. This also happen even if you link to `HTTP` -`mybank.example.com` and redirect to `HTTPs` -`https://mybank.example.com`, bad user can steal your credentials 

      > [!NOTE] *How Spring support HTTP Strict Transport Security* 
       > 			- One way for a site to be marked as a HSTS host is to **have the host preloaded into the browser**. 
       > 			- *The preload -> directive instructs the browser that the domain should be preloaded int Browser as AN HSTS DOMAIN* --> Details on on HSTS preload -> see 👁️ [hstspreload.org](https://hstspreload.org/) 
       > 			- Another way - **add the Strict-Transport-Security header** to the response. 
       > 			- SpringSe's default behaviour is to add the following header - instruct the browser to trete the domain as an HSTS host for a year 
       > 			`Strict-Transport-Security: max-age=31536000 ; includeSubDomains ; preload`	  
	- <span style="color:#4ee4b7">X-Frame-Options</span>
		- *The main purpose of header in helping security is what ?*
			- used to indicate whether or not a browser should be allowed to render a page in a `<frame>`, `<iframe>`, `<embed>` or `<object>`. 
			--> Sites can use this to avoid [clickjacking](https://owasp.org/www-community/attacks/Clickjacking) attacks, by ensuring that their content is not embedded into other sites.
		- *The case context you should need this header ?*
			- Using clever CSS styling, users could be tricked into clicking on something that they were not intending 
		- *Why it is a matter ?*
			- Base on the case context above, a user that logged into their bank might click a button that grants access to other users --> CALL A NAME "Clickjacking" ATTACK
	- <span style="color:#4ee4b7">X-XSS-Protection</span>
		- *The main purpose of header in helping security is what ?*
			- filter out reflected XSS attacks
			- a feature of Internet Explorer, Chrome, and Safari that stops pages from loading when they detect reflected cross-site scripting (XSS) attacks
		> [!WARNING]
		>- There is a warning of using this header, it work well to protect users of older web browser that do not yet support CSP. In some cases, this header can create XSS vulnerabilities in otherwise safe website source
		- 
		> [!RECOMMEND]
		>- Use a Content Security Policy (CSP) that disables the use of inline JavaScript.    
	- <span style="color:#4ee4b7">Content Security Policy (CSP)</span>
		- *The main purpose of header in helping security is what ?*
			- a security feature that is used to specify the origin of content that is allowed to be loaded on a website or in a web applications. 
			- it is an added layer of security that helps to detect and mitigate certain types of attacks including CROSS-SITE SCRIPTING XSS and data injection attacks. These attacks are which used for everything from data theft to site defacement to distribution of malware
		   >[!NOTE] CROSS-SITE SCRIPTING - XSS attacks  
		   >- CROSS-SITE SCRIPTING - XSS attacks: attacker attaches code onto legitimate website -> the website the will execute when the victim loads the website. That malicious code can be inserted in several ways. Popularly, added to the end of a url or posted directly onto a page that displays user-generated content
		   >![](https://www.cloudflare.com/img/learning/security/threats/cross-site-scripting/xss-attack.png)		
		- *The case context you should need this header ?*
			(Take the XSS attack as an example) 	Different types of cross-site scripting (2 types popular):
			- Reflected cross-site scripting: malicious code is added onto the end of the url of a website -> victim loads this link in the web browser -> browser execute the code injected into the url, for example :
			  ![](https://i.imgur.com/8ypkv6y.png)
			- Persistent cross-site scripting: happens on sites that let users post content that other users will see, such as a comment forum or social media --> if the site does not properly validate the inputs for user-generated content --> attacker insert code that other users' browsers will execute when the page loads
			  ![](https://i.imgur.com/bqiWu7P.png)
		- >[!Info] How Spring support CSP
		>	- CSP is not intended to solve all content injection vulnerabilities -> use CSP help reduce the harm caused by content injection attacks. But the first line of defense, web app authors should validate their input and encode their output
		>	- A web app use CSP by including one of the following HTTP headers in the response:
		> 		`Content-Security-Policy`
		>		 `Content-Security-Policy-Report-Only`
		>		*Two type of header will work to allow to execute the external trusted script* 
		>		 `Content-Security-Policy: script-src https://trustedscripts.example.com`
		>		 `Content-Security-Policy: script-src https://trustedscripts.example.com; report-uri /csp-report-endpoint/`
		>		 `Content-Security-Policy-Report-Only: script-src 'self' https://trustedscripts.example.com; report-uri /csp-report-endpoint/`
	- Other headers and details config to increase security, explore from here : 
		- ![](https://i.imgur.com/vNzLtrj.png)
		- 🔗 [OWASP Secure Headers Project](https://owasp.org/www-project-secure-headers/index.html#java)
		- 🔗 [HTTP Security Response Headers Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Headers_Cheat_Sheet.html)
		- 

> [!Note]
> To enhance new headers, custom headers, see here for more details : [servlet](https://docs.spring.io/spring-security/reference/servlet/exploits/headers.html#servlet-headers-custom)
 - Implement Security HTTP Response Header in SprSe: [ Security HTTP Response Headers](https://docs.spring.io/spring-security/reference/servlet/exploits/headers.html) 



#### HTTP Request
- All HTTP-based communication (communicate by using HTTP protocol), including static resources -- should be protected by using TLS.
> [!Caution]
> - As a framework, SPRING SECURITY does not handle HTTP connections --> does not provide support for `HTTPS` directly. 
> - However, it does provide a number of features that help with HTTPS USAGE

- Redirect to HTTPs
	- When a client uses HTTP -> let configure SprSe to redirect to HTTPs 
	*Redirect in servlet*
	![](https://i.imgur.com/uLGKJQh.png)
- Strict Transport Security default and config
	- ![](https://i.imgur.com/sQKYQxT.png)
- Proxy Server Configuration
	- *Why this come to the picture ?**
		- When using a proxy server --> important to ensure that you have configured your application properly
		- For example, many applications have a load balancer that responds to request for `https://example.com/` -> forwarding the request to an application server (another version of original server) `https://192.168.0.107` 
		- > [!FAIL]
        > ---> Without proper configuration. the application server cannot know that the load balancer exists (the other version of original server exists have the ip `192.168.0.107` example ) and treats the request as though `https://192.168.0.107:8080` was requested by the client
	- To fix - to make application aware of this new version of server to forward request for working -> you need to <span style="color:#aaee2b">configure you application server to be aware of the `X-Forwarded` headers</span>. *For example,* 
		- Tomcat uses `RemoteIpValve`
		- Jetty uses `ForwardedRequestCustomizer`
		- Spring Boot user can use `server.forward-headers-strategy` property to configure the application (see [Spring Boot documentation](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#howto.webserver.use-behind-a-proxy-server))
		- Spring user can use 
			[`ForwardedHeaderFilter`](https://docs.spring.io/spring-framework/reference/web/webmvc/filters.html#filters-forwarded-headers) with the Servlet stack 
			[`ForwardedHeaderTransformer`](https://docs.spring.io/spring-framework/reference/web/webflux/reactive-spring.html#webflux-forwarded-headers) with the Reactive stack

  - > [!INFO] Proxy
    - a server that acts as a gateway or intermediary between any device and the rest of the internet. 
    - A proxy accepts and forwards connection requests, then returns data for those requests. 
    - It uses the anonymous network id instead of actual IP address of client (means it hides the IP address of client), so that the actual IP address of client couldn’t be reveal
    ![](https://upload.wikimedia.org/wikipedia/commons/0/0f/Minh_h%E1%BB%8Da_v%E1%BB%81_Proxy.png)
    ![](https://images.viblo.asia/5bb9830d-184c-4aea-ba9e-a5e77f742444.png)

  - > [!INFO] Load Balancer
     - A practice of distributing computational workloads between two or more computer.
     - On the internet, load balancing is often employed to divide network traffic among several servers (one server but have many brothers and sisters server has the same configuration. Brothers ,sisters can be under the form of physical hardware or virtual on cloud)
     - This system design will help to reduces the strain on each server and make the servers more efficient, speeding up performance and reducing latency
     ![](https://i.imgur.com/IMitYIF.png)
     

---
## Servlet Applications
Spring Security integrates with the Servlet Container by using a standard `Servlet Filter` --> can work with any application runs in a Servlet Container
- Servlet Container
	- > [!INFO] Servlet Container
	  > [![](https://i.imgur.com/RBkQLnG.png)](https://www.baeldung.com/java-servlets-containers-intro)
### Started
- For supporting the Servlet Security, SprSe coordinate with Spring Boot to set the default configurations when you integrated for your servlet, so that it will have these following behaviors in runtime:
	![](https://i.imgur.com/HgLhA00.png)
- Let understand how SprBoot is coordinating with SprSe to achieve this by taking a look at <span style="color:#8d8d2a">Boot's security auto configuration</span> for example :...
	- 
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
	   - > [!NOTE] @EnableWebSecurity
        > 		- SprBoot <span style="color:#d7771d">adds any filter published as a @Bean to the application's filter chain</span> ---> using `@EnableWebSecurity` in conjunction with <span style="color:#d7771d">Spring Boot automatically registers SprSe's filter chain for every request</span>

	- 2 - Publishes a `UserDetailsService` @Bean with the generated account by the system, this account has username default is `user`, role default is `ROLE_USER`, password is the which randomly generated by the system that is logged to the consoled 
    > (has the format like this : "the password is `8e557245-73e2-4286-969a-ff57fe326336`")
    
	- 3 - Publishes an  [`AuthenticationEventPublisher`](https://docs.spring.io/spring-security/reference/servlet/authentication/events.html) @Bean for <span style="color:#7030a0">publishing authentication events</span>
- So when use need implement Security
	* Building <span style="color:#d4a216">Rest API</span> - need <span style="color:#d4a216">Authenticate a JWT</span> || Other <span style="color:#d4a216">Bearer Token</span>
	* Building <span style="color:#d4a216">Web Application, API Gateway</span>, Or <span style="color:#d4a216">BFF</span> 
		* need to login using OAuth 2.0 or OIDC
		* need to login using SAML 2.0
		* need to login using CAS
	* Need to manage 
		* Users in LDAP || Active Director, with Spring Data || with JDBC
		* Password
	* Security <span style="color:#d4a216">Protocol</span> which your application use to communicate (For servlet-based app -> SprSe supports HTTP, WebSockets)
	* For <span style="color:#d4a216">Authentication</span> + need <span style="color:#d4a216">authentication stateful || stateless</span>
	* For <span style="color:#d4a216">Authorization</span> 
	* <span style="color:#d4a216">Defense</span> - SprSe support defaults protections + additional protection you need



### Architecture
- Discusses SprSe's high-level architecture within Servlet based applications
#### A review of Servlet Filters
- The filters is really powerful in helping SprSe building a secure Servlet Application
	- Java Servlet Filter
		- > [!INFO] [Java Servlet Filter](https://openplanning.net/10395/java-servlet-filter)
        >	
         > ![](https://i.imgur.com/Hagt6IX.png)
         > ![](https://i.imgur.com/xYYQKQP.png)
         > ![](https://i.imgur.com/LCr9Tep.png)

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
	- 
		``` Java
			public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) {
			// do something before the rest of the application
		    chain.doFilter(request, response); // invoke the rest of the application
		    // do something after the rest of the application
		}
		```
		










#### DelegatingFilterProxy
   **What it really is ?**
- Spring provides a `Filter` implementation named `DelegatingFilterproxy` --> <span style="color:#d4a216">Allows Bridging between the Servlet container's lifecycle and Spring's ApplicationContext</span>
-  *<span style="color:#92d050">It is a servlet filter but allows passing control to specific Filter classes that have access to the Spring application context = registered as a `@Bean` - this filter class can be called as a Spring-managed bean that implements the Filter Interface </span>*
	*--> It is a Proxy for a standard Servlet Filter, delegating to a Spring-managed bean that implements the Filter interface.*
	
	--- 
- **What it really work ?**
	- The Servlet container allows registering `Filter` instances  by using its own standard. 
	- > [!NOTE] Note - Review Configuration `Filter`
		- > [!NOTE]
	     > - To use `filter` for servlet --> need to Config Servlet Filter - a way to register Filter information in file web.xml (`Deployment Descriptor File`) --> so that the Servlet Container knows which filters need to create instance at the first time deployed and then uses to filter any specific coming request based on filter-mapping url configuration.
	     > - Let see how to create `Filter` class and register them in file xml  
	     > ![](https://i.imgur.com/SOAN5pu.png)
	     > ![](https://i.imgur.com/BWKKjRo.png)
	     > ![](https://i.imgur.com/wA1xvbc.png)
	- BUT BY THIS WAY, these instances is not aware of Spring-defined Beans --> Instead, you can register <span style="color:#d4a216">`DelegatingFilterProxy` through the standard Servlet Container mechanisms but can delegate all the work to a Spring Bean that implements `Filter`</span> 
	- ![](https://i.imgur.com/ddBVANT.png)
	> - Explain how it works ?
	> 	- `DelegatingFilterProxy` looks up Bean Filter(0) from the ApplicationContext - the context in the Spring Framework --> then invokes Bean Filter(0).
	> 	- `DelegatingFilterProxy` Pseudo Code
	> 	 
	>	   ```Java
	>	      public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) {
	>      Filter delegate = getFilterBean(someBeanName); 1
	>		  delegate.doFilter(request, response); 2
	>		  }
	>		 ```
	>	 -  1 - Lazily get Filter that was registered as a Spring Bean (Lazily - mean load instance filter just when the first time call this Filter). For ex, DelegatingFilterProxy delegate is an instance of Bean Filter(0)
	>	 - 2 - Now, delegate work to the Spring Bean
	>	 > [!Note] Benefit of `DelegatingFilterProxy`
	>	 > - Allows delaying looking up Filter bean instance
	>	 >	--> This is important, because the *container needs to register the `Filter` Instances before the container can start up* -->  HOWEVER, with SPRING typically uses a `ContextLoaderListener` to load the Spring Beans --> but it not done until after the `Filter` instances need to be registered
	>	 >	
- > [!Info] Proxy Design Pattern & `DelegatingFilterProxy` more details
	- > [!Note] Proxy Design Pattern  
  > [![](https://i.imgur.com/BuqTfOr.png)](https://phanhoangminhluan.medium.com/proxy-pattern-and-delegatingfilterproxy-in-spring-framework-d79a10abfe30) 
  > [![](https://i.imgur.com/zfquBdj.png)](https://refactoring.guru/design-patterns/proxy)
  > 
  > What you can see here from Proxy idea ? 
  > --> Proxy is going to create a new proxy class - this class implement the same interface as the primary class, but this proxy instead implements its own logic code for overriding the abstract method interface -> it going to invoke the logic code from the primary class which implement the same interface.
  > But the power of proxy in here is that ---> allow you to execute before or after the primary logic of the primary class
  > 
  > --> This behavior can be called that proxy wrap the primary class
  > 
  > --> By taking of this benefits, we can use Proxy to 
  > 	🔹 Lazy initialization (virtual proxy)
  > 	🔹 Access control (protection proxy)
  > 	🔹 Local execution of a remote service (remote proxy)
  > 	🔹 Logging requests (logging proxy)
  > 	🔹 Caching request results (caching proxy)
  > 	
  > > [!Note] Let see how delegate take benefits of Proxy design Pattern 
  > ![](https://i.imgur.com/a3iWTM4.png)
  > ![](https://i.imgur.com/IjLoGUq.png)
  > ![](https://i.imgur.com/nrvP4vT.png)
  > ![](https://i.imgur.com/pLa0BYE.png)
  > > > [!SUMMARY]
  > > > When you create new `Filter` class that implements from `Filter` --> You do not have to declare in the Servlet Context but you have to register this filter as a` @Bean` to `ApplicationContext` --> Instead, The `DelegatingFilterProxy` - A servlet Standard Servlet Filter created already by the Spring boot --> This `DelegatingFilterProxy` filter is the one actual be declared in the Servlet Context --> When the request come, the `DelegatingFilterProxy` will get the instance of Filter above by searching its registered Bean Name of This Filter -> then invoke `delegate.doFilter(arguments)` and pass the arguments includes request, response, filterChain. 
  > > > ![](https://i.imgur.com/TXhbhtD.png) 
   > Reference : 
   > 🍎 [<span style="color:#3adf97">Overview and Need for DelegatingFilterProxy in Spring</span>](https://www.baeldung.com/spring-delegating-filter-proxy)
   >  🍎 [<span style="color:#3adf97">Proxy Pattern and DelegatingFilterProxy in Spring Framework</span>](https://www.baeldung.com/spring-delegating-filter-proxy)




#### FilterChainProxy
- Spring Security's Servlet support is contained within `FilterChainProxy`. 
- `FilterChainProxy` - <span style="color:#d4a216"> special `Filter` provided by SprSe - allows delegating to many `Filter` instance through `SecurityFitlerChain`</span>
- Because `FilterChainProxy` is A Bean  --> it typically wrapped in a `DelegatingFilterProxy`
	![](https://i.imgur.com/hK4RaSt.png)
> [!SUMMARY]
> - Meaning SprSe practice the security for upcoming request in the filter layer - to call which filter instances are used to check security -> the `FilterChainProxy` use `SecurityFitlerChain` - this will delegate the appropriate Security Filters )
#### SecurityFilterChain
- `SecurityFilterChain` - used by FilterChainProxy, <span style="color:#d4a216">help to determine which **Spring Security `Filter` Instances** should be invoked for the current request.</span>
- ![](https://i.imgur.com/lpdom8X.png)
- The `Security Filters` in `SecurityFilterChain` are typically `Beans` - but registered with `FilterChainProxy` instead of `DelegatingFilterProxy`
- Using `FilterChainProxy` provides you a number of <span style="color:#92d050">advantages to registering</span> <span style="color:#92d050">directly with the `Servlet Container` Or `DelegatingFilterProxy`</span>
	- First - provies a starting point for all of Spring `Security's Servlet support` --> if you try to troubleshoot with `Security's Servlet support` --> let <span style="color:#92d050">adding a debug point in `FilterChainProxy`</span>
		- ![](https://i.imgur.com/X3ROAFi.png)
		- ![](https://i.imgur.com/1yDXcFz.png)
		- ![](https://i.imgur.com/TnGCOIT.png)
		> 🍎(Find the Registered Spring Security Filters)[https://www.baeldung.com/spring-security-registered-filters] 

	- Second - `FilterChainProxy` - a central to SprSe usage 
		- --> it can <span style="color:#92d050">perform tasks that are not viewed as optional</span> (Ex: clears out the `SecurityContext` to avoid memory leaks)
		- --><span style="color:#92d050"> applies SprSe's `HttpFirewall` </span>to protect application against certain types of attacks
	- More - provides more flexibility in <span style="color:#92d050">DETERMINING WHEN A `SecurityFilterChain` SHOULD BE INVOKED</span>
	- > [!Note]
		- > In general **Filter Instances** are invoked based upon the URL alone - but with `FilterChainProxy` - it help to <span style="color:#92d050">determine invocation of which specific `SecurityFilterChain` based upon anything in the `HttpServletRequest` by using the `RequestMatcher` interface</span>
		- > Let see how is based on `RequestMatcher` idea ?
			* ![](https://i.imgur.com/pXXqDJ5.png)
			- > Regard the `SecurityFilterChain` like a box which contains only need filters for specific request url --> by this way, there are multi `SecurityFilterChain` for different urls
			- > <span style="color:#d4a216">Each `SecurityFilterChain` can have **different number of Filter instances**</span> (for ex, `SecurityFilterChain`(0) has 3 but `SecurityFilterChain`(n) has 4), can have <span style="color:#d4a216">**Zero security Filter**</span> for the case ignore security to a certain requests and can be <span style="color:#d4a216">**configured in isolation**</span>.
			>  
			- > Let explain how `SecurityFilterChain` is useful ?
			> - In the Multiple `SecurityFilterChain` figure above, `FilterChainProxy` decides which `SecurityFilterChain` - which big box filter should be used. By this way, 
			> - if url start with /api/*, /api/messages/ -> then only the first `SecurityFilterChain`(0) that matches is invoked.
			> - if url is /messages/ is requested - do not match on the `SecurityFilterChain`(0) pattern /api/** -> `FilterChainProxy` continues trying to each `SecurityFilterChain` --> assuming that no other `SecurityFilterChain` instances match --> then `SecurityFilterChainn`(n) is invoked.
			> ![](https://i.imgur.com/rRF7C7t.png)
			> Reference: 🍎 [Class FilterChainProxy](https://docs.spring.io/spring-security/site/docs/4.2.15.RELEASE/apidocs/org/springframework/security/web/FilterChainProxy.html)
			> ![](https://i.imgur.com/yEfffsd.png)
			> Reference: 🍎 [The Security Filter Chain](https://docs.spring.io/spring-security/site/docs/3.0.x/reference/security-filter-chain.html)
			> 
	- > [!Note] 
	  > In Spring Security, The `FilterChainProxy` has already the `DefaultSecurityFilterChain` for all the request upcoming,
	  > --> if you want custom config `SecurityFitlerChain` for specific urls let go to create you own `SecurityFilterChain` like this and config based on java config :
	  >   ![](https://i.imgur.com/VMpsqXR.png)
	  >   🍎 [# Introduction to Java Config for Spring Security](https://www.baeldung.com/java-config-spring-security))
	  >   You can also add new filter to the `SecurityFilterChain`
	  >   ![](https://i.imgur.com/8nBhZob.png)
	  >   ![](https://i.imgur.com/afYiuXQ.png)



#### Security Filters
- `Security Filters` are inserted into the `FilterChainProxy` with the `SecurityFilterChain API`
- Those filters can be used for purposes like authentication, authorization, exploit protection and more
- <span style="color:#d4a216">The Filers are executed in a specific order</span> to guarantee that they are invoked at the right time
	- For exam, Filter performs authentication should be invoked before the Filter that performs authorization
- 🍎 Want to know the ordering of Spring Security's Filter --> see  [`FilterOrderRegistration` code](https://github.com/spring-projects/spring-security/tree/6.2.0/config/src/main/java/org/springframework/security/config/annotation/web/builders/FilterOrderRegistration.java)
- <span style="color:#3adf97">Printing The Security Filters</span> - a kind of logging filters to the console
	- Useful to see the list of security `Filters` that are invoked for a particular request
	- 🍎 Let see how to config to log the filters information : [## Logging](https://docs.spring.io/spring-security/reference/servlet/architecture.html#servlet-logging)
- <span style="color:#3adf97">Adding a Custom Filter</span> to the Filter Chain
	- Their might be times you want to add a custom `Filter` to the existing security filter chain --> 🍎 let see example of add new filter : [Adding a Custom Filter to the Filter Chain](https://docs.spring.io/spring-security/reference/servlet/architecture.html#adding-custom-filter)
#### Handling Security Exceptions
- The `ExceptionTranslationFilter` allows translation of `AccessDeniedException` and `AuthenticationException` into HTTP responses.
- `ExceptionTranslationFilter` is inserted into the `FilterChainProxy` as one of the Security Filters.
- ![](https://i.imgur.com/Skd8I7S.png)
- 🍎 Let see in big picture: [Handling Security Exceptions](https://docs.spring.io/spring-security/reference/servlet/architecture.html#servlet-exceptiontranslationfilter)

#### Saving Requests Between Authentication
- Why you need ? Look back to the picture of `Handling Security Exceptions`, <span style="color:#d4a216">when a request has no authentication and is for a resource that requires authentication --> a need to save the request for the authenticated resource to later re-request after authentication is Successfully</span>.
- In Spring, the place to store the request-`HttpServletRequest` is to use a [`RequestCache`](https://docs.spring.io/spring-security/reference/servlet/architecture.html#requestcache) implementation
- The filter which uses the [`RequestCache`](https://docs.spring.io/spring-security/reference/servlet/architecture.html#requestcache) to save the `HttpServletRequest` is [`RequestCacheAwareFilter`](https://docs.spring.io/spring-security/site/docs/6.2.0/api/org/springframework/security/web/savedrequest/RequestCacheAwareFilter.html)  
- But the default request cache get implemented is `HttpSessionRequestCache`. --> Let see the code to show how to customize the `RequestCache` implementation that is used to check the `HttpSession` for a saved request if the parameter named `continue` is present 
	```Java
	@Bean
	DefaultSecurityFilterChain springSecurity(HttpSecurity http) throws Exception {
	HttpSessionRequestCache requestCache = new HttpSessionRequestCache();
	requestCache.setMatchingRequestParameterName("continue");
	http
		// ...
		.requestCache((cache) -> cache
			.requestCache(requestCache)
		);
	return http.build();
	}
	```
#### Logging
- Printing the Security Filters to track for DEBUG and TRACE
- Let see the interesting example :
	- Example where a user tries to make a `POST` request to a resource that has [CSRF protection](https://docs.spring.io/spring-security/reference/servlet/exploits/csrf.html) enabled without the CSRF token. With no logs, the user will see a 403 error with no explanation of why the request was rejected. However, if you enable logging for Spring Security, you will see a log message like this:

		```text
		2023-06-14T09:44:25.797-03:00 DEBUG 76975 --- [nio-8080-exec-1] o.s.security.web.FilterChainProxy        : Securing POST /hello
		2023-06-14T09:44:25.797-03:00 TRACE 76975 --- [nio-8080-exec-1] o.s.security.web.FilterChainProxy        : Invoking DisableEncodeUrlFilter (1/15)
		2023-06-14T09:44:25.798-03:00 TRACE 76975 --- [nio-8080-exec-1] o.s.security.web.FilterChainProxy        : Invoking WebAsyncManagerIntegrationFilter (2/15)
		2023-06-14T09:44:25.800-03:00 TRACE 76975 --- [nio-8080-exec-1] o.s.security.web.FilterChainProxy        : Invoking SecurityContextHolderFilter (3/15)
		2023-06-14T09:44:25.801-03:00 TRACE 76975 --- [nio-8080-exec-1] o.s.security.web.FilterChainProxy        : Invoking HeaderWriterFilter (4/15)
		2023-06-14T09:44:25.802-03:00 TRACE 76975 --- [nio-8080-exec-1] o.s.security.web.FilterChainProxy        : Invoking CsrfFilter (5/15)
		2023-06-14T09:44:25.814-03:00 DEBUG 76975 --- [nio-8080-exec-1] o.s.security.web.csrf.CsrfFilter         : Invalid CSRF token found for http://localhost:8080/hello
		2023-06-14T09:44:25.814-03:00 DEBUG 76975 --- [nio-8080-exec-1] o.s.s.w.access.AccessDeniedHandlerImpl   : Responding with 403 status code
		2023-06-14T09:44:25.814-03:00 TRACE 76975 --- [nio-8080-exec-1] o.s.s.w.header.writers.HstsHeaderWriter  : Not injecting HSTS header since it did not match request to [Is Secure]
		```
	*--> It becomes clear that the CSRF token is missing and that is why the request is being denied.*
- <span style="color:#d4a216">To configure your application to log all the Security events, let add the following to application.properties in Spring Boot file.</span>
	```
	logging.level.org.springframework.security=TRACE
	```










### Authentication
   2 parts
- The Servlet Authentication Architecture - concentrate on abstract describing the architecture without much discussion on how it applies to concrete flows
- The Authentication Mechanisms - concentrate on concrete ways in which users can authenticate
#### Part 1 - Discuss Servlet Authentication Architecture
   - This is the expands on Architecture Section Above -  [Servlet Security: The Big Picture](https://docs.spring.io/spring-security/reference/servlet/architecture.html#servlet-architecture), describe the main architectural components of SprSe's used in Servlet Authentication, include following here :
	   - > SecurityContextHolder
	   - > SecurityContextHolder
	   - > Authentication (Obj)
	   - > GrantedAuthority
	   - > AuthenticationManager
	   - > ProvideManager
	   - > AuthenticationProvider
	   - > Request Credentials with AuthenticationEntryPoint
	   - > AbstractAuthenticationProcessingFilter
#### Part 2 - Authentication Architecture
- ![](https://i.imgur.com/aWbkAYo.png)
- [![](https://i.imgur.com/LSFK7AQ.png)](https://docs.spring.io/spring-security/reference/servlet/authentication/architecture.html#servlet-authentication-abstractprocessingfilter)
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
> 🍎 Learn more: [Introduction to Spring Method Security](https://www.baeldung.com/spring-security-method-security)
- When using username/password based authentication then  `GrantedAuthority` instances are usually loaded by the [`UserDetailsService`](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/user-details-service.html#servlet-authentication-userdetailsservice)
- <span style="color:#81ed0c">Usually, the GrantedAuthority Objects are application-wide permissions - they are not specific to a given domain object</span>
  > If you would likely have 
  

##### AuthenticationManager
- Is the API - defines how SprSe's Filters perform authentication process to build `Authentication` obj - this means that the  `AuthenticationManager` help to specify which Authentication Mechanism is used for authentication

- > [!NOTE]
  >---
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
	- > Each `AuthenticationProvider` has an <span style="color:#d4a216">opportunity to indicate</span> that authentication should be successfull || fail || indicate it cannot make a decision -> then allow a downstream  `AuthenticationProvider` to decide
	- > If none of the configured `AuthenticationProvider` instances can authenticate -> <span style="color:#d4a216">authentication fails with exception</span> `ProviderNotFoundException` (mean - `AuthenticationException` indicates the `ProviderManager` was not configured to support the [[Software Knowledges/Programming Language/Java/Java Framework/5 - Java Spring Security/Spring Security 6.2.0#^Authentication-Implementing-Classes\|type of Authentication]] that was passed into it)
	- > Each `AuthenticationProvider` knows how to perform a specific type of authentication
		- > For ex, one `AuthenticationProvider` might be able use to validate a username/password (like `DaoAuthenticationProvider`) but another might be able to authenticate a SAML assertion (like )
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
- Another existing AuthenticationProviders which Spre Support [[Software Knowledges/Programming Language/Java/Java Framework/5 - Java Spring Security/Spring Security 6.2.0#^Authentication-Providers\|#^Authentication-Providers]]




##### Request Credentials with AuthenticationEntryPoint
- This is used to send an HTTP response that requests credentials from a client
- If the request is to a resource -> SprSe does not need to provide HTTP response that requests credentials from the client (since they are already included)
- <span style="color:#d4a216">Another case to show the useful of Request Credentials with `AuthenticationEntryPoint` is that when a client makes an unauthenticated request to a resource -> the are not authorized to access -> an implementation of `AuthenticationEntryPoint` is used to request back the credentials from the client by how -> it might perform a redirect to a login page and respond with an [WWW-Authenticate](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/basic.html#servlet-authentication-basic) header, or take other action</span>.


##### AbstractAuthenticationProcessingFilter
- It is used as a base Filter for authenticating a user's credentials
- Before the credentials can be authenticated --> SprSe typically requests the CREDENTIALS by using `AuthenticationEntryPoint` (in the case request to the resource but not login)
- If the credentials of client exists -> the `AbstractAuthenticationProcessingFilter` can authenticate any authentication requests that are submitted to it
- [![](https://i.imgur.com/LSFK7AQ.png)](https://docs.spring.io/spring-security/reference/servlet/authentication/architecture.html#servlet-authentication-abstractprocessingfilter)
#### Username/Password Authentication
- The most common ways to authenticate a user by validating a username and password
- 🍎 Let see the default configuration for Username/Password authentication here: [Username/Password Authentication](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/index.html)
- > [!Note]
> - To enhance the custom configuration and learn more detail for username-password authentication work, you can consider those use cases following here: 
>	-  The built-in mechanisms for reading a username and password from `HttpSerletRequest`
>	-  **Form Login works** - The login form template which is integrated into the Spring Security by default to require the user credentials for authentication. You can see how to custom this form too.
>	-  **HTTP Basic authentication works** - indicate when the request is caught access denied -> then auto redirect to the form which require login by provide the sign for require login name WWW-`Authenticate` header
>		-  `**DaoAuthenticationProvider` works**
>	-  **Storage Mechanisms** - Store authenticated users 
>		-  in memory 
>		-  in a database 
>		-  in LDAP
>	-  Custom authentication by <span style="color:#d4a216">publishing an `AuthenticationManager` bean</span>
>	-  Customize the global `AuthenticationManager`
>	-  🍎 Reference: [Username/Password Authentication](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/index.html)
- <span style="color:#81ed0c">Publish an `AuthenticationManager` bean is doing what ?</span>
	- Allow you for custom authentication by customizing the `AuthenticationManager` inside
	- *For example, in case you want to authenticate users via a REST API instead of using Form Login*
	- <span style="color:#d4a216">To custom authentication --> so what are the things you need to config in `AuthenticationManger`</span>
		- <span style="color:#d4a216">config `ProviderManager` instance</span> by using which `Authentication Provider` instances you want to call.
		- <span style="color:#d4a216">set `UserDetailsService`</span> (<span style="color:#ff0000">The instruction for custom `UserDetailService` is here: ....</span>)
		- <span style="color:#d4a216">set `PasswordEncoder` mechanism</span> - which password encoder type you want to encode password 
		> Blueprint
		- ![](https://i.imgur.com/5IQx5AB.png)
		- <span style="color:#d4a216">Finally - config the invocation for using new custom authentication</span>, for example:
			- Just using for specific `RestController`: [example](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/index.html#publish-authentication-manager-bean)
			- Use the new custom `AuthenticationManagerBuilder` in the global application - for all login requirements> -> using `AuthenticationManagerBuilder`: 
				- [![](https://i.imgur.com/hn7A3WX.png)](https://www.javadevjournal.com/spring-security/spring-security-custom-authentication-provider/)
				- ![](https://i.imgur.com/gYDX2Rr.png)

##### Storage Mechanisms (Mechanisms for storage and reading a username, password for authentication)
 * SprSe supports included storage mechanisms:
	- Simple Storage with [In-Memory Authentication](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/in-memory.html#servlet-authentication-inmemory)
		- > You can use `InMemoryUserDetailsManager` which implements the `UserDetailsService` interface to provide support for username/password based authentication that is stored in memory
	- Relational Databases with <span style="color:#d4a216">JDBC Authentication</span>
		- SprSe provide support retrieve username-password-based authenticaion by using JDBC - use the class `JdbcDaoImpl` which implements `UserDetailsService` (can regard the `JdbcDaoImpl` similar to the Repository but for the Entity kind of User Account which includes required fields are username and password, the feature of the class `JdbcDaoImpl` is that it uses SQL Scripting. ![](https://i.imgur.com/MpVeJzk.png)). --> To use of `JdbcDaoImpl` need `JdbcUserDetailsManager` which extends `JdbcDaoImpl` to provide management of `UserDetails` through the `UserDetailsManager` interface.
		- 🍎 Let see how to use here: 
			- [JDBC Authentication](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/jdbc.html#servlet-authentication-jdbc)
			- [Spring Security: Exploring JDBC Authentication](https://www.baeldung.com/spring-security-jdbc-authentication)
			- <iframe width="300" height="200" src="https://www.youtube.com/embed/d7ZmZFbE_qY?si=fDduLrrUsnxPRZAs" title="Spring Security JDBC: How to authenticate against a database in Spring Boot" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
	- > [!Caution]
	> - This technique have the drawbacks if we want to customize the database schema, or if we want to use a different database vendor.
	> - ✅✅✅ Strongly Recommend to use another technique which also work similar to JDBC Authentication but flexible in working with another database vendor and database schema named "[**Authentication with a Database-backed UserDetailsService**](https://www.baeldung.com/spring-security-authentication-with-a-database)" - [[Software Knowledges/Programming Language/Java/Java Framework/5 - Java Spring Security/Spring Security 6.2.0#^Custom-data-store-UserDetailsService\|#^Custom-data-store-UserDetailsService]] 
	- Custom data stores with [UserDetailsService](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/user-details-service.html#servlet-authentication-userdetailsservice)^Custom-data-store-UserDetailsService
	- > [!Info] 
	  >- `UserDetails` - interface: 
	  >	- provides core information about the current user authentication
	  >	- Implementations are not used directly by SprSe for Security purposes (The Implementation classes are: ![](https://i.imgur.com/87WqGZo.png)) --> These implementations simply store user information - which is later encapsulated into `Authentication` obj + This allows non-security related user information (email, telephone, etc) to be stored in a convenient location.
	  >	- To custom the user details for creating implementation of `UserDetails` -> let see 
	  >	- 🍎  [User](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/core/userdetails/User.html "class in org.springframework.security.core.userdetails") for a reference implementation (which you might like to extend r use in your code)
	  >	- 🍎 [CustomUserRepositoryUserDetailsService.java](https://github.com/spring-projects/spring-security-samples/blob/main/servlet/spring-boot/java/authentication/username-password/user-details-service/custom-user/src/main/java/example/CustomUserRepositoryUserDetailsService.java)
	  >- `UserDetailsService` - interface
	  >	- Core interface which loads user-specific data
	  >	- `UserDetailsService` is used by `DaoAuthenticationProvider` for retrieving a username, a password, and other attributes for authenticating with a username and password
	  >	- It work as a user DAO
	  >	- Requires only one read-only method for reading the `UserDetails` by method name `loadUserByUsername(String username)` (username indicate the field that you based on to query and get the account entity from database) - finding the user.
	  >	- 🍎 Reference: [UserDetailsService](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/core/userdetails/UserDetailsService.html)
	- <span style="color:#d4a216">So how custom user data stores with `UserDetailsService` ?</span>
		- create your CustomUser entity
		- retrieveing a user from DAO class using Spring Data be extending the `JPARepository` interface
		- create your CustomUserDetails which implements `UserDetails`
		- create CustomUserDetailsService which Implement `UserDetailsService` and override the method `loadUserByUsername(String username)`
		- 🍎 Reference: [Authentication with a Database-backed UserDetailsService](https://www.baeldung.com/spring-security-authentication-with-a-database)
	- <span style="color:#81ed0c">LDAP storage with</span> [LDAP Authentication](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/ldap.html#servlet-authentication-ldap)
		- LDAP - Lightweight Directory Access Protocol - often used by organizations as a central repository for user information + as an authentication service
		- It is used to store the role information for application users
		- > [!Note]
			> - SprSe'LDAP-based authentication - used by SprSe when it is configured to acccept a username/password for authentication
		    > - BUT despite using a username and password for authentication -> it do not use UserDetailsService - because in **bind authentication** -> LDAP does not return the password for performing validation of password
		- > [!Caution] 🔥 Prerequisites
		  >	To apply LDAP with SprSe, require being familiar with LDAP before
          >	
		  >   🍎 Reference: [OpenLDAP](https://www.zytrax.com/books/ldap/),
		  >	[Java LDAP documentation](https://docs.oracle.com/javase/jndi/tutorial/ldap/connect/config.html)
		  >	[LDAP](https://dienhuynhit.blogspot.com/2013/05/ldap-phan-1.html)






#### Persistence
- 🤔 Purpose of using Persistence Authentication ?
	- It is a kind of "persistent login session" - the user able to return to your website and not have to log in again.
	- 🙀 When an unauthenticated user requests to the protected resource -> let redirect the user to the login form for the credentials requirements. But the key is here is avoid requiring credentials from clients for every requests coming. ---> Your app need something call persistence to keep the status authenticated of your user --> Persistence Authentication Come Up -<span style="color:#d4a216">-> In SprSe, upon authenticating the user, the user is **associated to a new session id** --> **Subsequent request include the session cookie** - which is used to authenticate the user for the remainder of the session (while the session is not expired yet). </span>
- So the mechanism for applying this technique ?
	- In SprSe, the association of the user to future requests is made USING [**`SecurityContextRepository`**](https://docs.spring.io/spring-security/site/docs/6.2.0/api/org/springframework/security/web/context/SecurityContextRepository.html)
	  > This will use to persisting a `SecurityContext` between requests - but the persistence mechanism used will depend on the implementation, but most commonly the `HttpSession` will be used to store the context
{ #Security-Context-Repository-Doing-What}

	- Default implementation of `SecurityContextRepository` IS [`DelegatingSecurityContextRepository`](https://docs.spring.io/spring-security/site/docs/6.2.0/api/org/springframework/security/web/context/DelegatingSecurityContextRepository.html) --> delegates to the following:
{ #default-implementation-of-SecurityContextRepository}

		- [`HttpSessionSecurityContextRepository`](https://docs.spring.io/spring-security/reference/servlet/authentication/persistence.html#httpsecuritycontextrepository) 
		  > - this associates the `SecurityContext` to the `HttpSession` (You can replace this with another implementation)
		- [`RequestAttributeSecurityContextRepository`](https://docs.spring.io/spring-security/reference/servlet/authentication/persistence.html#requestattributesecuritycontextrepository) 
		  > - Stores the `SecurityContext` on a `ServletRequest.setAttribute(String, Object)` -> so that it can by restored when different dispatch types occur. But it will not be available on subsequent requests. (Unlike [`HttpSessionSecurityContextRepository`](https://docs.spring.io/spring-security/site/docs/6.2.0/api/org/springframework/security/web/context/HttpSessionSecurityContextRepository.html "class in org.springframework.security.web.context") - this filter has no need to persist the `SecuirtyContext` on the response being committed because the `SecuirtyContext` will not be availble for subsequent requests for `RequestAttributeSecurityContextRepository`)
			  >  => help to make sure the `SecurityContext` is available for a single request that occurs across dispatch types that may clear out the `SecuirityContext`
			  >> *for example, assume that a client makes a request - is authenticated - but then an error occurs --> depending on the servlet container implementation - the error means that any `SecurityContext` that was established - the existing `SecurityContext` is cleared out -> then the error dispatch is made*. --> Result in that there is no `SecurityContext` established - error page cannot use the `SecuirityContext` for authorization || displaying the current user   (unless the SecuirityContext is persisted somehow )
			- > [!SUMMARY]
		      >   - RequestAttributeSecurityContextRepository là một cơ chế để lưu trữ và khôi phục SecurityContext trong trường hợp yêu cầu request (authenticated request) đi qua các loại dispatch có thể làm mất `SecurityContext`
              >   - Khi một yêu cầu mới được xử lý hoặc dispatch, RequestAttributeSecurityContextRepository kiểm tra xem có SecurityContext đã được lưu trữ trong yêu cầu (request) không --> Nếu có, nó sẽ khôi phục `SecurityContext` từ thuộc request.getAttribute() và sử dụng nó cho việc xác thực và hiển thị thông tin người dùng liên quan --> Giúp đảm bảo rằng `SecurityContext` không bị mất đi qua các loại dispatch và vẫn có sẵn để sử dụng trong quá trình xử lý yêu cầu
			- > [!INFO] Dispatch
				> - Hiểu là chuyển hướng yêu cầu từ một phần của hệ thống đến một thành phần khác để xử lý tiếp theo. Liên quan chuyển tiếp yêu cầu từ một trang hoặc một phần của ứng dụng web đến một trang hoặc một phần khác :
			  > - Trong Servlet API, dispatch có thể xảy ra thông qua phương thức `RequestDispatcher.forward()` hoặc `RequestDispatcher.include()`
	- [Configure DelegatingSecurityContextRepository](https://docs.spring.io/spring-security/reference/servlet/authentication/persistence.html#delegatingsecuritycontextrepository)
	- In SprSe, for persisting the `SecurityContext` between requests in `SecurityContextRepository`-> the SprSe support 2 kinds of Filter: 
		- [SecurityContextPersistenceFilter](https://docs.spring.io/spring-security/reference/servlet/authentication/persistence.html#securitycontextpersistencefilter) - <span style="color:#91819c">responsible for **persisting** the `SecurityContext` between requests using the `SecurityContextRepository`</span>
		- [SecurityContextHolderFilter](https://docs.spring.io/spring-security/reference/servlet/authentication/persistence.html#securitycontextholderfilter) - <span style="color:#91819c">responsible for **loading** the `SecurityContext` between requests using the `SecurityContextRepository`</span>
		> *In Spring Security 6, the `SecurityContextPersistenceFilter` and `SessionManagementFilter` are not set by default. In addition to that, any application should only have either `SecurityContextHolderFilter` or `SecurityContextPersistenceFilter` set, never both.*

#### Authentication Persistence && Session Management
- <span style="color:#d4a216">About Authentication Persistence</span>
  > Once you have got an application that is authenticating requests -> it is important to consider how that resulting authentication will be persisted and restored on future requests.
  > Let explore how to persist authenticated user account through Session :
  > ![](https://i.imgur.com/mMP6XTv.png)
1. Session Management's Components
	- `SecurityContextHolderFilter` || `SecurityContextPersistenceFilter` ()
	- `SessionManagementFilter` - [[Software Knowledges/Programming Language/Java/Java Framework/5 - Java Spring Security/Spring Security 6.2.0#^Session-Management-Filter\|#^Session-Management-Filter]]
	- > [!Caution]
	  > In Spring Security 6, the `SecurityContextPersistenceFilter` and `SessionManagementFilter` are not set by default. --> The default behavior is that [the `SecurityContextHolderFilter`](https://docs.spring.io/spring-security/reference/servlet/authentication/persistence.html#securitycontextholderfilter) will only read the `SecurityContext` from `SecurityContextRepository` and populate it in the `SecurityContextHolder`
	  > In addition to that, any application should only have either `SecurityContextHolderFilter` or `SecurityContextPersistenceFilter` set, never both.
	  > 
	- `SessionManagementFilter`
{ #Session-Management-Filter}

		- The [`SessionManagementFilter`](https://docs.spring.io/spring-security/reference/servlet/authentication/session-management.html#the-sessionmanagementfilter) checks the contents of the `SecurityContextRepository` - [[Software Knowledges/Programming Language/Java/Java Framework/5 - Java Spring Security/Spring Security 6.2.0#^Security-Context-Repository-Doing-What\|#^Security-Context-Repository-Doing-What]] against the current contents of the `SecurityContextHolder` to determine whether a user has been authenticated during the current request - typically by a non-interactive authentication mechanism, such as `pre-authentication` or `remember-me`
		- > [!Note]
	     > - In Spring Security 6, the default is that authentication mechanisms themselves must invoke the `SessionAuthenticationStrategy` --> the `SessionManagementFilter` is not used by default as like Spring Security 5 (*because of problem is that when use `SessionManagementFilter`, the `HttpSession` must be read for every request to detect if a current user authenticated*), therefore, some methods from the `sessionManagement` DSL will not have any effect.
	     > - ![](https://i.imgur.com/ndOA06l.png)
2. Customizing Where the Authentication Is Stored
	- Default - SprSe stores security context in HTTP Session --> some reasons you want to customize that you may want 
		- To call individual setters on the `HttpSessionSecurityContextRepository` instance
		- To store the security context in a cache or database to enable horizontal scaling
	- [Config how](https://docs.spring.io/spring-security/reference/servlet/authentication/session-management.html#customizing-where-authentication-is-stored)**  --> *First - create an implementation of `SecurityContextRepository` (the default implementation of this interface is [[Software Knowledges/Programming Language/Java/Java Framework/5 - Java Spring Security/Spring Security 6.2.0#^default-implementation-of-SecurityContextRepository\|#^default-implementation-of-SecurityContextRepository]] or use an existing implementation like `HttpSessionSecurityContextRepository` --> then set it in `HttpSecurity`*
	- Extension for Custom Authentication
		- Storing the Authentication manually
		- Properly Clearing an Authentication
		- Configuring Persistence for Stateless Authentication 
		- > [!Info] Stateless Authentication
		  > a way to verify users by **having much of the session information** such as user properties stored on the client side
3. Understanding <span style="color:#d4a216">Require Explicit Save</span>
	- Note, the **Require Explicit Save** has been referred in [`SecurityContextHolderFilter`](https://docs.spring.io/spring-security/site/docs/6.2.0/api/org/springframework/security/web/context/SecurityContextHolderFilter.html) terminology
	- So what does Require Explicit Save mean ? 
		-> Use the `SecurityContextHolderFilter` instead of `SecurityContextPersistenceFilter` for only read the `SecurityContext` from `SecurityContextRepository` and populate it in the `SecurityContextHolder`. 
		 > It will not automatically save the `SecurityContext` when using  `SecurityContextPersistenceFilter` (because this cause the [problem](https://docs.spring.io/spring-security/reference/servlet/authentication/session-management.html#requireexplicitsave) out of keep track of the state to determine if a save `SecurityContext` is necessary)
		---> removes ambiguity and improves performance by only requiring writing to the `SecurityContextRepository` (i.e. `HttpSession`) when it is necessary
    - [How it works with `SecurityContextHolderFilter`](https://docs.spring.io/spring-security/reference/servlet/authentication/session-management.html#how-it-works-requireexplicitsave)
4. Using `SecurityContextHolderStrategy`- [See](https://docs.spring.io/spring-security/reference/servlet/authentication/session-management.html#use-securitycontextholderstrategy)
   > Because of `SecurityContextHolder` - there is one strategy(The `SecurityContext`) per classloader instead of one per application context
    > To address -> Wire `SecurityContextHolderStrategy` from the application context --> help to look up the strategy(The `SecurityContext`) from `SecurityContextHolder`
-<span style="color:#d4a216"> About Session Management</span>
6. Configuring Concurrent Session Control - [See](https://docs.spring.io/spring-security/reference/servlet/authentication/session-management.html#ns-concurrent-sessions)
 > *For Q: I want to place some constrains on a single user's ability to login to my app for the purpose not allow for logging in multiple times ?*
6. Detecting Timeouts - [See](https://docs.spring.io/spring-security/reference/servlet/authentication/session-management.html#_detecting_timeouts)
 > Session expire on their own - sesions có thể tự động hết hạn, but there is nothing that needs to be done to ensure that a security context gets removed. -> SprSe support to detect when a session has expired and take specific actions that you indicates.
	- Những lúc cần cấu hình session expire time vd điển hình là cho các ứng dụng ngân hàng, mỗi lần đăng nhập một phiên có thời gian timeout -> khi expired là phải quay lại login để tạo session làm việc mới.
	- Thông thường when session timeout, or expired -> session will automatically be deleted on browser. But in the case of log out without closing browser, and session still not expired -> result in session cookie is not cleared when you invalidate the session and will be resubmitted even if the user has logged out --> need [configure logout to clear the session cookie](https://docs.spring.io/spring-security/reference/servlet/authentication/session-management.html#clearing-session-cookie-on-logout)
7. <span style="color:#d4a216">Configuring Session Fixation Protection</span> - [See](https://docs.spring.io/spring-security/reference/servlet/authentication/session-management.html#_configuring_session_fixation_protection)
	- [Session fixation](https://en.wikipedia.org/wiki/Session_fixation) attacks - a malicious attacker to create a session by accessing a site -> then persuade another user to log in with the same session (by sending them a link containing the session identifier as a parameter, for example). 
	- <span style="color:#91819c">---> Spring Security protects against this automatically by creating a new session or otherwise changing the session ID when a user logs in.</span>

🍎 **<span style="color:#81ed0c">Spring Session Reference</span>** - [Spring Session](https://docs.spring.io/spring-session/reference/index.html)


#### Remember Me
- ><span style="color:#91819c">This technique will work like to generate access token after login successfully and then save this token in the cookie of browser --> the server will use this to automatically login for every request as well as when the client close the browser - reopen the website.</span>
- Introduction
	- Remember-me || persistent-login authentication --> refers to web sites being able to remember the identity of a principal between sessions
	- The technique to accomplish is by sending a cookie to the browser -> with the cookie being detected during future sessions -> causing automated login to take place. (Session - A **web session** is a series of contiguous actions by a visitor on an individual website within a given time frame )
	- ---> <span style="color:#d4a216">SprSe provides the necessary hooks for these operations to take place and has 2 concrete remember-me implementations</span>
		- Once uses hashing to preserve the security of cookie-based tokens - [[Software Knowledges/Programming Language/Java/Java Framework/5 - Java Spring Security/Spring Security 6.2.0#^Simple-Hash-Based-Token-Approach\|#^Simple-Hash-Based-Token-Approach]]
		- Once uses a database || other persistent storage mechanism (im memory for example, in memory database like H2) to store the generated tokens
		- > [!Caution] These both implementations require of using `UserDetailsService` 
		  > If use an authentication prover that does not use a `UserDetailsService` (LDAP provider for ex) -> it does not work unless have a `UserDetailsService` bean in application context
- Simple Hash-Based Token Approach
{ #Simple-Hash-Based-Token-Approach}

	- This approach uses hashing to achieve a useful remember-me strategy
	- Remember-me token is valid only for the period specified and only if username, password, and key do not change
	- >[!Caution] Potential security issue
	- a captured remember-me token is usable from any user agent until such time as the token expires (remember-me token của người đã được xác thực bị lấy đi và sử dụng trong máy của một người nào khác) -> result in example the bad guy can easily change your password --> invalidate all remember-me tokens on issue
- Persistent Token Approach
	- This approach is based on the article titled [http://jaspan.com/improved_persistent_login_cookie_best_practice](http://jaspan.com/improved_persistent_login_cookie_best_practice), 
	- By this appraoch, the username is not included in the cookie, to prevent exposing a valid login name unecessarily.
- Remember-Me Interfaces and Implementations
	- Remember-me - be used with `UsernamePasswordAuthenticationFilter`
	              - be implemented through hooks in the `AbstractAuthenticationProcessingFilter` superclass
	              - be used within `BasicAuthenticationFilter`
	>  🍎 Reference: [`RememberMeServices`](https://docs.spring.io/spring-security/site/docs/6.2.0/api/org/springframework/security/web/authentication/RememberMeServices.html)
	- `TokenBasedRememberMeServices` - [See](https://docs.spring.io/spring-security/reference/servlet/authentication/rememberme.html#_tokenbasedremembermeservices)
		- The implementation of `RememberServices` -> supports the simpler approach described in [Simple Hash-Based Token Approach](https://docs.spring.io/spring-security/reference/servlet/authentication/rememberme.html#remember-me-hash-token)
		- Working 
			- [`TokenBasedRememberMeServices`](https://docs.spring.io/spring-security/site/docs/6.2.0/api/org/springframework/security/web/authentication/rememberme/TokenBasedRememberMeServices.html) use the user information by retrieving username and password from] `UserDetailsService` --> then generates `RememberMeAuthenticationToken`
			- `RememberMeAuthenticationProvider`uses both `RememberMeAuthenticationToken` and `UserDetailsService` for signature comparison purposes  
		    > => the key/token is shared between `RememberMeAuthenticationProvider` & `TokenBasedRememberMeServices`
		- `TokenBasedRememberMeServices` also implements SprSe's `LogoutHandler` interface so that it can be used with `LogoutFilter` to have the cookie cleared automatically.
#### Handling Logouts
- By default, SprSe stans up a /logout endpoint -> no additional code is neccessary
- The Logouts Cover these use cases:
	![](https://i.imgur.com/SkOfAql.png)
- Understanding Logout’s Architecture
	- When include the `spring-boot-starter-security` dependency or use the `@EnableWebSecurity` annotation --> SprSe add its LOGOUT SUPPORT --> provide default respond both to **GET /logout** and **POST /logout**
		- Get /logout - SprSe displays a logout confirmation page
		- Post /logout - SprSe perform the following default operations using a series of [`LogoutHandler`](https://docs.spring.io/spring-security/site/docs/6.2.0/api/org/springframework/security/web/authentication/logout/LogoutHandler.html)s:
		     > Implementing Classes of `LogoutHandler` Interface
		     > ![](https://i.imgur.com/Il4d3B3.png) 
		     > <span style="color:#d4a216">**This will called by `LogoutFilter`**</span>
			<span style="color:#d4a216">- Invalidate the HTTP session 
			- Clear the `SecurityContextRepository`
			- Clean up any `RememberMe` authentication
			- Clear out any saved CSRF token
			- Fire a `LogoutSuccessEvent`
			- Exercise default `LogoutSuccessHandler` - redirects to /login/logout, when all above success completed.</span>
	- 🍎 Reference : [Understanding Logout’s Architecture](https://docs.spring.io/spring-security/reference/servlet/authentication/logout.html#logout-java-configuration)
	- SprSe suport for Customizing Logout URIs - [See](https://docs.spring.io/spring-security/reference/servlet/authentication/logout.html#customizing-logout-uris)


#### Authorization Events - [See](https://docs.spring.io/spring-security/reference/servlet/authorization/events.html)
- Take examples, for each authorization that is denied -> an `AuthorizationDeniedEvent` is fired, Also -> possible to fire and `AuthorizationGrantedEvent` for authorizations that are granted.
**==> <span style="color:#d4a216">To listen for these events, you must publish an `AuthorizationEventPublisher`**</span>

### Authorization
<span style="color:#91819c">- This section concentrates to configure your application's authorization rules
- Consider attaching authorization rules to request URIs and methods
- Consider how Spring Security authorization works</span>
#### Authorization Architecture
- Describes the SprSe architecture that applies to authorization
- Authorities
	- Authentication has referred that all the implementations of Authentication Interface  <span style="color:#d4a216"> store a list of `GrantedAuthority` objects</span> - represent the authorities (quyền hạn) that have been granted to the principal
	- The `GrantedAuthority` objects -> <span style="color:#d4a216">inserted into the Authentication obj by the `AuthenticationManager`</span> & are later <span style="color:#d4a216">read by `AccessDecisionManager` instances when making authorization decisions</span>
	- ![](https://i.imgur.com/a1AVLqW.png)
	- ![](https://i.imgur.com/deoduir.png)
	- > [!Note] Method `String getAuthority();`
	  > This method is used by an `AuthorizationManager` instance to obtain a precise **String** **representation of the `GrantedAuthority`**-> this String - a `GrantedAuthority` (thằng đại diện cho thằng này là cái string được trả về từ `String getAuthority();`) can be "read" easily by most `AuthorizationManager` implementations
	- > [!Caution] String is null
	  > If a `GrantedAuthority` cannot be precisely represented as a String - be considered "complex" -> `getAuthority()`will be must return null.
	  > the "complex" of `GrantedAuthority` would be an implementation that stores a List of operations and authority thresholds(for example, instead contains one Role, the implementation stores a list different roles) --> representing this complex GrantedAuthority as a String (this refer have to read the array) would be DIFFICULT --> As a result, the getAuthority() method should return null.
	  > ==> any AuthorizationManager want to support the specific GrantedAuthority (for ex: for role USER || ADMIN retrieve from the list which contain both USER, ADMIN) -> need to support the specific `GrantedAuthority` implementation (for example: implementing classes `OAuth2UserAuthority`, `OcidcUserAuthority`,..) to understand its "complex" contents.
	  > 	
	  > <span style="color:#d4a216">RESOVE HOW ?</span>
	   > SprSe includes one concrete `GrantedAuthority` implementation: `SimpleGrantedAuthority` --> this implementation lets any user-specified `String` be converted into a `GrantedAuthority` (sẽ kiểu đọc từ danh sách các role, lấy ra một role và convert sang Sring). All `AuthenticationProvider` instances included with the security architecture use `SimpleGrantedAuthority` to populate the `Authentication` object
	   > Configuration:  
		- > ``` Java
		  > public class MyDatabaseUserDetailsService implements UserDetailsService {
		  >  UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
		  >     User user = userDao.findByUsername(username);
		  >     List<SimpleGrantedAuthority> grantedAuthorities = user.getAuthorities().map(authority -> new SimpleGrantedAuthority(authority)).collect(Collectors.toList()); // (1)
		  >     return new org.springframework.security.core.userdetails.User(user.getUsername(), user.getPassword(), grantedAuthorities); // (2)
		  >  }
		  >}
		  >```

	- 🍎 Reference: [Authorities](https://docs.spring.io/spring-security/reference/servlet/authorization/architecture.html#authz-authorities)
- The Invocation Handling - Handle the spring security exceptions
	> - **SprSe provides interceptors** (sự chen giữa/chen ngang) --> that control access to secure (such as method invocations or web request). 
	> - **A pre-invocation** decision on whether the invocation is allowed to proceed - this is made by `AuthorizationManager` instances
	> - **A post-invocation** decisions on whether a given value may be returned - this is made by `AuthorizationManager` instances
	- <span style="color:#81ed0c">So, what is `AuthorizationManager` and how it make access control decision</span>
	---
	- The `AuthorizationManger` - what is ?
		- `AuthorizationManager` supersedes both `AccessDecisionManger` and `AccessDecisionVoter`([[Software Knowledges/Programming Language/Java/Java Framework/5 - Java Spring Security/Spring Security 6.2.0#^AccessDecisionManager-and-AccessDecisionVoter\|#^AccessDecisionManager-and-AccessDecisionVoter]])
		- > [!Caution]
		> Applications that customize an `AccessDecsionManager` or `AccessDecisionVoter` need to change to use `AuthorizationManager` (because from Spr 6.0.0 the `AccessDecisionManager` has been deprecated) 
		- `AuthorizationManager`s **are called by SprSe's** 
			-  <span style="color:#ff0000">request based authorization - Authorize `HttpServletRequests`</span>
			-  <span style="color:#ff0000">method-based authorization - Method Security</span> 
			- <span style="color:#ff0000"> message-based authorization - WebSocket Security</span>
		- `AuthorizationManager` are responsible for making final access control decisions. 
		- The interface contains two methods
		```java
		AuthorizationDecision check(Supplier<Authentication> authentication, Object secureObject);
		
		default AuthorizationDecision verify(Supplier<Authentication> authentication, Object secureObject)
		        throws AccessDeniedException {
		    // ...
		}
		```
		 - `check` method is passed all the relevant information it needs in order to make an authorization decision - information: authentication, secured object
			 - the `check` method in implementations (các class triển khai `AuthorizationDecision`) expected return 
				 - POSITIVE `AuthorizationDecision` access is GRANTED
				 - NEGATIVE `AuthorizationDecision` access is DENIED
				 - NULL `AuthorizationDecision` decision is ABSTAIN
				 > (compare to `AccessDecisionVoter` which the implementations vote method return 3 kind of static field name: ACCESS_ABSTAIN, ACCESS_GRANTED, ACCESS_DENIED - [[Software Knowledges/Programming Language/Java/Java Framework/5 - Java Spring Security/Spring Security 6.2.0#^AccessDecisionVoter-static-fields-named\|#^AccessDecisionVoter-static-fields-named]])
		- `verify` method call check, throw `AccessDeniedException` when check method return NEGATIVE `AuthorizationDecision`
	- Delegate-based `AuthorizationManager` Implementations - how does ?
		- > To help `AuthorizationManager` to control access decision --> this interface has its own a composition(tổ hợp) of `AuthorizationManager` implementations in the following picture here:  
		- ![](https://i.imgur.com/ybFnvs4.png)
		- ![](https://i.imgur.com/iUbDgaM.png)
		- SprSe ships with a delegating `AuthorizationManager` - collaborate with individual `AuthorizationManager`s
		- <span style="color:#d4a216">Match request</span> - `RequestMatcherDelegatingAuthorizationManager` - an implementation of `AuthorizationManager` interface --> match the request with the most appropriate delegate `AuthorizationManager` (delegates to a specific [`AuthorizationManager`](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/authorization/AuthorizationManager.html "interface in org.springframework.security.authorization") based on a [`RequestMatcher`](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/web/util/matcher/RequestMatcher.html "interface in org.springframework.security.web.util.matcher") evaluation)
		- <span style="color:#d4a216">Method Security</span> - can use 
			- `AuthorizationManagerBeforeMethodInterceptor` - A `MethodInterceptor` which uses a `AuthorizationManager` to determine if an `Authentication` may invoke the given `MethodInvocation` (the method that is assigned for specific authority - ex: a function only allow role admin)
			- `AuthorizationManagerAfterMethodInterceptor`- A MethodInterceptor which can determine if an Authentication has access to the result of an MethodInvocation using an AuthorizationManager
			> The mechanic that Method Security - these interceptors apply is the AOP - [Aspect Oriented Programming with Spring](https://docs.spring.io/spring-framework/reference/core/aop.html)
			- 🍎 Reference: [Method Security](https://docs.spring.io/spring-security/reference/servlet/authorization/method-security.html)
		- AuthorityAuthorizationManager - [See](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/authorization/AuthorityAuthorizationManager.html)
			- > An AuthorizationManager that determines if the current user is authorized by evaluating if the Authentication contains a specified authority.
			- > Return positive `AuthorizationDecision` should the `Authentication` contain any of the configured authorities (authorities which specified during the configuration by `@Secured({ "ROLE_VIEWER", "ROLE_EDITOR" })`, `@RolesAllowed("ROLE_VIEWER")` annotations for ex). It will return a negative `AuthorizationDecision` otherwise.
		- AuthenticatedAuthorizationManager - [See](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/authorization/AuthenticatedAuthorizationManager.html)
			- > An AuthorizationManager that determines if the current user is authenticated.
			- > Can be used to differentiate between anonymous, fully-authenticated and remember-me authenticated users.
	- Custom Authorization Manager
		- > The purpose of Authentication Manager is given the approach to make decision access on authenticated user base on it's authorities(roles)
		- > 🍎 Reference: [Custom Authorization with Spring Boot](https://insource.io/blog/articles/custom-authorization-with-spring-boot.html)
	- Hierarchical Roles
		- requirement that a particular role in an application should automatically "include" other roles.
		- The use of a role-hierarchy allows you to configure which roles (or authorities) should include others.
		-  An extended version of Spring Security’s `RoleVoter`, `RoleHierarchyVoter`, is configured with a `RoleHierarchy`, from which it obtains all the "reachable authorities" which the user is assigned.
		- <span style="color:#91819c">Confiugration: Hierarchical Roles Configuration</span>
		
			``` Java
				@Bean
			static RoleHierarchy roleHierarchy() {
			    RoleHierarchyImpl hierarchy = new RoleHierarchyImpl();
			    hierarchy.setHierarchy("ROLE_ADMIN > ROLE_STAFF\n" +
			            "ROLE_STAFF > ROLE_USER\n" +
			            "ROLE_USER > ROLE_GUEST");
			    return hierarchy;
			}
			
			// and, if using method security also add
			@Bean
			static MethodSecurityExpressionHandler methodSecurityExpressionHandler(RoleHierarchy roleHierarchy) {
				DefaultMethodSecurityExpressionHandler expressionHandler = new DefaultMethodSecurityExpressionHandler();
				expressionHandler.setRoleHierarchy(roleHierarchy);
				return expressionHandler;
			}	
			```
  - > [!Info] `AccessDecisionManger` and `AccessDecisionVoter`
{ #AccessDecisionManager-and-AccessDecisionVoter}

		- > [!Info] `AccessDecisionManger`
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
		- > [!Info] `AccessDecisionVoter` - Voting-Based `AccessDecisionManager` Implementations
			> - While users can implement their own `AccessDecisionManager` to control all aspects (liên quan khái niệm AOP - [Aspect Oriented Programming with Spring](https://docs.spring.io/spring-framework/reference/core/aop.html) ) of authorization.
			> - Spring Security includes several `AccessDecisionManager` implementation are based on voting.
			>  ![](https://i.imgur.com/Up3oQgo.png)
			>  > The picture is Voting Decision Manager - describes the relevant classes
			> - <span style="color:#91819c">By using this approach --> a series of `AccessDecisionVoter` implementations (like RoleVoter, AuthenticatedVoter, or your new CustomVoter,...) are polled on an authorization decision ---> then the `AccessDecisionManager` decides whether or not to throw an `AccessDeniedException` based on its assessment of the votes.</span>
			>   ![](https://i.imgur.com/QIwNsvX.png)
			> - Note that, the concrete/the voting implementations of `AccessDecisionVoter` interface have to return an int - with possible values being **reflected in the `AccessDecisionVoter` static fields named**:
{ #AccessDecisionVoter-static-fields-named}

			> 	- <span style="color:#91819c">ACCESS_ABSTAIN - A voting implementation returns this when if it has no opinion on an authorization decision</span>
			> 	- <span style="color:#91819c">ACCESS_DENIED || ACCESS_GRANTED (1 of 2): A voting implementation returns either of them if it does have an opinion .</span>
			> - In the picture, we can see that `AccessDecisionManager` has three concrete - regard as `AccessDecisionVoter` - to give the decision of access denied for current authentication obj: 
			>   ![](https://i.imgur.com/Ky5VzlP.png)
			>   - The `ConsensusBased` implementation grants or denies access based on the consensus of non-abstain votes (phiếu trắng)
			>   - The `AffirmativeBased` implementation grants access if one or more `ACCESS_GRANTED` votes were received
			>   - The `UnanimousBased` provider expects unanimous `ACCESS_GRANTED` votes in order to grant access, ignoring abstains.
			> - 🍎 Reference
			>   [Voting-Based AccessDecisionManager Implementations](https://docs.spring.io/spring-security/reference/servlet/authorization/architecture.html#authz-voting-based)
			> - > [!Info] RoleVoter
			>> - This treats configuration attributes as role names -> <span style="color:#d4a216">votes to grant access</span> if the user has been assigned that role
			>> - It votes if any ConfigAttribute begins with the ROLE_ prefix
			>> - It votes to grant access if there is `GrantedAuthority` 's representation return a String (from the `getAuthority()` method) exactly equal to one or more `ConfigAttributes` that that start with the ROLE_ prefix. If no -> vote to deny access, if `ConfigAttributes` begins with ROLE_ (not thing following) -> voter abstains. 
			>>   ![](https://i.imgur.com/OwDw2Cf.png)
			>> - 🍎 Reference: Custom Voters 
			>> - [Custom AccessDecisionVoters in Spring Security](https://www.baeldung.com/spring-security-custom-voter)
			>> - [Configuring Spring Boot’s Method Level Security To Check Only The Required Permission](https://medium.com/@adammcg97/configuring-spring-boots-method-level-security-to-use-a-non-cached-source-to-check-for-7df7317127d2)
			>> - [Spring Security: Custom Access Decision Voter](https://blog.jdriven.com/2019/10/spring-security-custom-access-decision-voter/)
		   > - > [!Caution]  
		   > `AccessDecisionVoter` and `AccessDecisionManager` are not recommend to use, instead you should use `AuthorizationManager` above, this section is included for historical purpose


### OAuth2
#### Overview
![](https://i.imgur.com/b9inffD.png)
> Protocol flow - the abstract OAuth 2.0 flow illustrated in Figure above describe the interaction between the 4 roles and its steps.
#### OAuth2 Log In
- **The OAuth 2.0 Login** lets an application have **users log into the application** by **using** their **existing account** at an **OAuth 2.0 Provider - such as GITHUB |OR| OpenID Connect 1.0 Provider (such as Google)**. 	
##### Core Configuration
- The Spring Boot provides full auto - configuration for OAuth2.0 Login.
	- See the example shows how to configure the OAuth2 Login by using Google as the Authentication Provider [See](https://docs.spring.io/spring-security/reference/servlet/oauth2/login/core.html#oauth2login-sample-initial-setup)
	- <span style="color:#d4a216">Summary</span> for implement default configuration of OAuth2 Login 
	- 1 - Completing the "**Obtain OAuth2.0 credentials**" instructions to get your app new OAuth Client with **credentials consisting of a Client ID & Client Secret**.
		- example for Google Credentials :
		   ![](https://i.imgur.com/MUa10Oq.png)
	- 2 - Setting Redirect URI - the path in the application that that the end-user's user-agent is redirected back to after they have authenticated with Google and have granted access to the OAuth Client
		- ![](https://i.imgur.com/2Sc46wp.png)
	- 3 - Configure application.yml - when you have a new OAuth Client at step 1 --> configure the application to use the OAuth Client for the authentication flow. The configuration follows the format: 
		- ![](https://i.imgur.com/Cd5xtTE.png)
	- 4 - Boot up the Application - default the Spring Boot 2.x app will be launch to go to localhost 8080 and then redirected to the default auto-generated login page which displays a link for Google. 
		- ![](https://i.imgur.com/VgirjD6.png)
- Custom more configuration for Spring Boot 2.x OAuth Client properties by following templates [See](https://docs.spring.io/spring-security/reference/servlet/oauth2/login/core.html#oauth2login-boot-property-mappings)
- CommonOAuth2Provider pre-defines
	- CommonOAuth2Provider pre-defines a set of default client properties for a number of well known providers includes: Google, GitHub, Facebook, and Okta. In addition, the authorization-uri, token-uri, user-info-uri do not change often for a provider ---> Therefore, with common Oauth2 Provider, Spring Se support default values ---> developer just set up client-id and client-secret.
		![](https://i.imgur.com/7EP0WNY.png)
	- 👕 Custom your own OAuth2Provider/registrationId - for cases where you may want to specify a different `registrationId` such as goolge-login --> *config the provider property*
		- ![](https://i.imgur.com/cgNMV88.png)
	- ![](https://i.imgur.com/jp0PwFk.png)

- Configuring Custom Provider Properties - support multi-tenancy
	- For the purpose providing supports multi-tenancy for some OAuth2.0 Provider require.
	- `Multi-tenancy` - means using different protocol endpoints for each tenant/sub-model.
		For example: an OAuth Client registration with Okta is assigned to a specific sub-domain and have their own protocol endpoints.
	- 👕 Let go to config custom provider properties: `spring.security.oauth2.client.provider._[providerId]_`
		- ![](https://i.imgur.com/wPjyCVE.png)
- Overriding Spring Boot 2.x Auto-Configuration
	- The auto-configuration class for <span style="color:#00b0f0">**OAuth Client support is `OAuth2ClientAutoConfiguration`. Performing the following tasks:**</span>
		- <span style="color:#00b0f0">Registers a `ClientRegistrationRepository` `@Bean` which composed of `ClientRegistration`(s) (like Github, Facebook, ... other OAuth2 Providers) from OAuth Client properties.</span>
		- <span style="color:#00b0f0">Registers a `SecurityFilterChain` @Bean and enables OAuth2.0 Login through `httpSecurity.oauth2Login()`</span>
			- ![](https://i.imgur.com/m3X7nMo.png)
		-  ⭐ To Override the auto-configuration --> do so in the following ways: 
			- [Register a ClientRegistrationRepository @Bean](https://docs.spring.io/spring-security/reference/servlet/oauth2/login/core.html#oauth2login-register-clientregistrationrepository-bean)
			- [Register a SecurityFilterChain @Bean](https://docs.spring.io/spring-security/reference/servlet/oauth2/login/core.html#oauth2login-provide-securityfilterchain-bean)
			- [Completely Override the Auto-configuration](https://docs.spring.io/spring-security/reference/servlet/oauth2/login/core.html#oauth2login-completely-override-autoconfiguration)
	- Java Configuration without Spring Boot 2.x - if you not able to use Spring Boot 2.x and would like to configure one of the pre-defined providers in CommonOAuth2Provider like Google for ex -> [See](https://docs.spring.io/spring-security/reference/servlet/oauth2/login/core.html#oauth2login-javaconfig-wo-boot)

##### Advanced Configuration
- HttpSecurity.oauth2Login() provides a number of configuration options for customizing OAuth 2.0 Login 
- <span style="color:#d4a216">The **main configuration** options are **grouped into their protocol endpoint counterparts** includes :</span>
	```Java
		@Configuration
		@EnableWebSecurity
		public class OAuth2LoginSecurityConfig {
			@Bean
			public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
				http
					.oauth2Login(oauth2 -> oauth2
					    .authorizationEndpoint(authorization -> authorization
					            ...
					    )
					    .redirectionEndpoint(redirection -> redirection
					            ...
					    )
					    .tokenEndpoint(token -> token
					            ...
					    )
					    .userInfoEndpoint(userInfo -> userInfo
					            ...
					    )
					);
				return http.build();
			}
		}
	```
- The authorization process uses <span style="color:#d4a216">2 authorization server endpoints (HTTP resources)</span> :
	- [`authorizationEndpoint`](https://datatracker.ietf.org/doc/html/rfc6749#section-3.1) - used by the client (your application) to obtain authorization from the resource owner through user-agent redirection.
	- [`tokenEndpoint`](https://datatracker.ietf.org/doc/html/rfc6749#section-3.2) - used by the client to exchange an authorization grant  for an access token
- The authorization process also uses <span style="color:#d4a216">one client endpoint</span> :
	- `redirectionEndpoint` - used by the authorization server to return response that contain authorization credentials to the client/your server through the resource owner user-agent.
- The OpenID Connect Core 1.0 specifications defines the [UserInfo Endpoint](https://openid.net/specs/openid-connect-core-1_0.html#UserInfo) as follows: 
	- `userInfoEndpoint` - is an OAuth 2.0 Protected Resource that returns <span style="color:#00b0f0">claims about the authenticated end-user</span>.
	- 👕 ---> To obtain the requested claims about the end-user 
		  ---> the client makes a request to the UserInfo Endpoint by using an access token which obtained through OpenID Connect Authentication. 
			![](https://i.imgur.com/UNpoVMg.png)
		  ---> these claims are normally represented by a JSON object that contains a collection of name-value pairs for the claims
			![](https://i.imgur.com/2ceOxc4.png)
	- 🍎 Reference: [UserInfo Endpoint](https://openid.net/specs/openid-connect-core-1_0.html#UserInfo)
- See the complete OAuth2 login configuration options
	```Java
			@Configuration
			@EnableWebSecurity
			public class OAuth2LoginSecurityConfig {
			
				@Bean
				public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
					http
						.oauth2Login(oauth2 -> oauth2
						    .clientRegistrationRepository(this.clientRegistrationRepository())
						    .authorizedClientRepository(this.authorizedClientRepository())
						    .authorizedClientService(this.authorizedClientService())
						    .loginPage("/login")
						    .authorizationEndpoint(authorization -> authorization
						        .baseUri(this.authorizationRequestBaseUri())
						        .authorizationRequestRepository(this.authorizationRequestRepository())
						        .authorizationRequestResolver(this.authorizationRequestResolver())
						    )
						    .redirectionEndpoint(redirection -> redirection
						        .baseUri(this.authorizationResponseBaseUri())
						    )
						    .tokenEndpoint(token -> token
						        .accessTokenResponseClient(this.accessTokenResponseClient())
						    )
						    .userInfoEndpoint(userInfo -> userInfo
						        .userAuthoritiesMapper(this.userAuthoritiesMapper())
						        .userService(this.oauth2UserService())
						        .oidcUserService(this.oidcUserService())
						    )
						);
					return http.build();
				}
			}
	```
- The following sections go into more detail on each of the configuration options available:
	- [OAuth 2.0 Login Page](https://docs.spring.io/spring-security/reference/servlet/oauth2/login/advanced.html#oauth2login-advanced-login-page) - Let go to customize the default login page which has already shows each configured OAuth Client with its `ClientRegistration.clientName` as a link
	
	 - [Redirection Endpoint](https://docs.spring.io/spring-security/reference/servlet/oauth2/login/advanced.html#oauth2login-advanced-redirection-endpoint) - customize the call back endpoint which the Authorization server uses for returning the Authorization Response(contains the authorization credentials) to the client through Resource Owner user-agent.
	
	 - 📌📌📌📌[UserInfo Endpoint](https://docs.spring.io/spring-security/reference/servlet/oauth2/login/advanced.html#oauth2login-advanced-userinfo-endpoint) - provide a number of configuration options includes [Mapping User Authorities](https://docs.spring.io/spring-security/reference/servlet/oauth2/login/advanced.html#oauth2login-advanced-map-authorities) ; [OAuth 2.0 UserService](https://docs.spring.io/spring-security/reference/servlet/oauth2/login/advanced.html#oauth2login-advanced-oauth2-user-service) ; [OpenID Connect 1.0 UserService](https://docs.spring.io/spring-security/reference/servlet/oauth2/login/advanced.html#oauth2login-advanced-oidc-user-service)
	
	- [ID Token Signature Verification](https://docs.spring.io/spring-security/reference/servlet/oauth2/login/advanced.html#oauth2login-advanced-idtoken-verify) -OpenID Connect 1.0 Authentication introduces the [ID Token](https://openid.net/specs/openid-connect-core-1_0.html#IDToken), which is a security token that contains Claims about the Authentication of an End-User by an Authorization Server when used by a Client for example
		- ![](https://i.imgur.com/f3cKu7R.png)
	
	- [oauth2login-advanced-oidc-logout](https://docs.spring.io/spring-security/reference/servlet/oauth2/login/advanced.html#oauth2login-advanced-oidc-logout)
	
#### OAuth2 Client 
**Overview**
- The OAuth2 Client feature support as the Client Role which pre-defined in the OAuth 2.0 Authorization Framework.
- At high-level the core feature available are: 
	- Authorization Grant - a credential representing the resource owner's authorization(to access its protected resources) used by the client to obtain an access token), under here Spring support 4 grant types : 
		- Authorization Code - [details concept](https://datatracker.ietf.org/doc/html/rfc6749#section-1.3.1) 
		- Implicit - use access token [details concept](https://datatracker.ietf.org/doc/html/rfc6749#section-1.3.2)
		- Client Credentials - [details concept](https://datatracker.ietf.org/doc/html/rfc6749#section-1.3.4)
		- Resource Owner Password Credentials - use username/password [details concept](https://datatracker.ietf.org/doc/html/rfc6749#section-1.3.3)
		<span style="color:#00b0f0">Extension:</span> 
		- - Refresh Token - [details concept](https://datatracker.ietf.org/doc/html/rfc6749#section-1.5)
		- JWT Bearer
		^authorization-grant
	- Client Authentication support
		- JWT Bearer - [details concept](https://datatracker.ietf.org/doc/html/rfc7523#section-2.2)
	- HTTP Client support
		- WebClient integration for Servlet Environments (for requesting protected resource) - details concept
- In Spring, for providing supports core features above, the `HttpSecurity.oauth2Client()` DSL provides a number of configuration options for customizing the core components used by OAuth 2.0 Client.
- <span style="color:#7030a0">In here, the `OAuth2AuthorizedClientManager` - responsible for managing the authorization(ore re-authorization) of an OAuth 2.0 Client. + Collaboration with one or more `OAuth2AuthorizedClientProvider`(s) </span> 
---
- 🍎 Reference: [OAuth2 Client](https://docs.spring.io/spring-security/reference/servlet/oauth2/client/index.html)
##### Core Interfaces and Classes 
- Describe the OAuth2 core interfaces and classes that Spring Security offers
1. <span style="color:#d4a216">ClientRegistration</span> 
	- A representation of a client registered with an OAuth2.0 or OpenID Connect 1.0 Provider. 
	- It holds information such as: client id, client secret, [authorization grant type](#^authorization-grant), redirect URI, scope(s), authorization URI, token URI, and other details. 
	- See `ClientRegistration` and its properties are defined as follows [here](https://docs.spring.io/spring-security/reference/servlet/oauth2/client/core.html#oauth2Client-client-registration)
2. <span style="color:#d4a216">ClientRegistrationRepository</span> - serves as a repository for OAuth 2.0/OpenID Connect 1.0 `ClientRegistration`(s)
	- Client registration information is ultimately stored and owned by the associated Authorization Server. ---> This repository provides the ability to retrieve a subset of the primary client registration information (which is also stored with the `AuthorizationServer`).
	- Spring Boot auto-configuration binds each of the properties under `spring.security.oauth2.client.registration.[registrationId]` (example like google.client-id, google.client-secret, google.redirectUri ) to an instance of [ClientRegistraion](https://docs.spring.io/spring-security/reference/servlet/oauth2/client/core.html#oauth2Client-client-registration) ---> then composed each of the `ClientRegistration` instances within a `ClientRegistrationRepository`
	- <span style="color:#d4a216">Note</span> - the default implementation of `ClientRegistrationRepository` is `InMemoryClientRegistrationRepository` (Cause we do not need to save `ClientRegistration` in database so let save it in memory)
	- 🍎 Reference: [ClientRegistrationRepository](https://docs.spring.io/spring-security/reference/servlet/oauth2/client/core.html#oauth2Client-client-registration-repo)
3. <span style="color:#d4a216">OAuth2AuthorizedClient</span>
	- Client đã được Resource Owner đồng ý cấp quyền truy cập của vào thông tin của end-user đó. 
	- OAuth2AuthorizedClient is a representation of an Authorized Client. A client is considered to be authorized when the end-user (the Resource Owner) has granted authorization to the client to access its protected resources.
	-  Its <span style="color:#00b0f0">main purpose</span> is <span style="color:#00b0f0">associating an `OAut2AccessToken` + optional `OAuth2RefreshToken` to a `ClientRegistration` (Client) and `Resource Owner`</span>(who is the Principal end-user that granted the authorization)
	- ![](https://i.imgur.com/RqjuRww.png)
1. <span style="color:#d4a216">OAuth2AuthorizedClientRepository && OAuth2AuthorizedClientService</span>
	- `OAuth2AuthorizedClientRepository` - responsible for persisting `OAuth2AuthorizedClient`(s) between web requests 
	-  `OAuth2AuthorizedClientService` - manage `OAuth2AuthorizedClient`(s) at the application-level.
	- <span style="color:#7030a0">Can use `OAuth2AuthorizedClientRepository` or `OAuth2AuthorizedClientService` to look up an `OAuth2AccessToken` associated with a client so that it can be used to initiate a protected resource request</span> [Example](https://docs.spring.io/spring-security/reference/servlet/oauth2/client/core.html#oauth2Client-authorized-repo-service)
	- > [!NOTE] Custom Auto configuration
	> The default implementation of `OAuth2AuthorizedClientService` is `InMemoryOAuth2AuthorizedClientService`, which stores `OAuth2AuthorizedClient` objects in-memory. 
    > 👕 The application allow you to override and register a custom OAuth2AuthorizedClientRepository or OAuth2AuthorizedClientService as @Bean (example like use `JdbcOAuth2AuthorizedClientService` to persist `OAuth2AuthorizedClient` instances in a database)
5. <span style="color:#d4a216">OAuth2AuthorizedClientManager && OAuth2AuthorizedClientProvider</span>
	- The `OAuth2AuthorizedClientManager` - responsible for the overall management of `OAuth2AuthorizedClient`(s)
		- The primary responsibilities includes:
			- <span style="color:#00b0f0">Authorizing (or re-authorizing) an OAuth 2.0 Client</span>, by using an `OAuth2AuthorizedClientProvider`.
			- <span style="color:#00b0f0">Delegating the persistence</span> of an `OAuth2AuthorizedClient`, typically by using an `OAuth2AuthorizedClientService` or `OAuth2AuthorizedClientRepository`.
			- <span style="color:#00b0f0">Delegating</span> to an `OAuth2AuthorizationSuccessHandler` <span style="color:#00b0f0">when an OAuth 2.0 Client has been successfully authorized</span> (or re-authorized).
			- <span style="color:#00b0f0">Delegating</span> to an `OAuth2AuthorizationFailureHandler` <span style="color:#00b0f0">when an OAuth 2.0 Client fails to authorize (or re-authorize)</span>.
	- The `OAuth2AuthorizedClientProvider` implements a strategy for authorizing or re-authorizing an OAuth 2.0 Client. 
		- <span style="color:#00b0f0">Implementations typically an authorization grant type</span> such as: `authorization_code`, `client_credentials`, and others.
	- <span style="color:#d4a216">Note</span> 
		- <span style="color:#9a7db0">The default implementation of `OAuth2AuthorizedClientManager` is `DefaultOAuth2AuthorizedClientManager` - associated with an `OAuth2AuthorizedClientProvider`</span>--> support multiple authorization grant types using delegation-based composite
		- <span style="color:#9a7db0">Code shows an example of how to configure and build an `OAuth2AuthorizedClientProvider` composite that provides support for the `authorization_code`, `refresh_token`, `client_credentials`, and `password` authorization grant types</span> [See](https://docs.spring.io/spring-security/reference/servlet/oauth2/client/core.html#oauth2Client-authorized-manager-provider)
	- `DefaultOAuth2AuthorizedClientManager` 	
		- <span style="color:#00b0f0">Authorization succeeds</span> --> delegates to the `OAuth2AuthorizationSuccessHandler` --> by default, this saves the `OAuth2AuthorizedClient` through OAuth2AuthorizedClientRepository (this can be customize)
		- <span style="color:#00b0f0">Re-Authorization failure</span> (ex: refresh toke no longer valid) - the previously saved `OAuth2AuthorizedClient` be removed from the `OAuth2AuthorizedClientRepository` (this can be customize)
		- <span style="color:#00b0f0">Associated with a `contextAttributesMapper`</span> of type `Function<OAuth2AuthorizeRequest, Map<String, Object>> ` --> responsible for <span style="color:#00b0f0">mapping attribute(s) from the `OAuth2AuthorizeRequest`</span> to a `Map` of attributes to be <span style="color:#00b0f0">associated to the `OAuth2AuthorizationContext`</span> --> useful when you need to supply an `OAuth2AuthorizedClientProvider` with required (supported) attributes.
			> Example: PasswordOAuth2AuthorizedClientProvider requires resource owner’s `username` and `password` to be available in OAuth2AuthorizationContext.getAttributes(). 
		   > Code snipped example of the `contextAttributesMapper` [See](https://docs.spring.io/spring-security/reference/servlet/oauth2/client/core.html#oauth2Client-authorized-manager-provider)
		- It is <span style="color:#00b0f0">designed to be used within the context of a `HttpSerletRequest`</span> -> when operating outside of a `HttpServlet` context -> use `AuthorizedClientServiceOAuth2AuthorizedClientManager` instead ?
##### OAuth2 Authorization Grants
This section describes Spring Security's support for [authorization grants](#^authorization-grant)
Spring support these types of authorizations grants
1. Authorization Code Grant
	- ![](https://i.imgur.com/2T0O9Qh.png)
	- Introduction 
		^intro-auth-code-grant
		- The authorization code grant type is <span style="color:#00b0f0">used to obtain both access tokens & refresh tokens</span> + is <span style="color:#00b0f0">optimized</span> for <span style="color:#00b0f0">confidential clients</span>
		- This a redirection-based flow --> <span style="color:#00b0f0">client</span> must be<span style="color:#00b0f0"> capable of</span> <span style="color:#00b0f0">interacting with the resource owner's user-agent</span> and capable of <span style="color:#00b0f0">receiving incoming request (via redirection) from the authorization server</span>.
		- [More details](https://datatracker.ietf.org/doc/html/rfc6749#section-4.1)
		---> To achieve obtaining authorization code flow mention in [introduction](#^intro-auth-code-grant). The OAuth2 Authorization framework proposed the idea of using [Authorization Request/Response](https://tools.ietf.org/html/rfc6749#section-4.1.1) to achieve protocol flow for the Authorization.
	<span style="color:#00b050">***---> Explore how Spring support:***</span>
	- Initiating the Authorization Request
		- The `OAuth2AuthorizationRequestRedirectFilter`
			- Uses an `OAuth2AuthorizationRequestResolver` to resolve an `OAuth2AuthorizationRequest` + initiate the Authorization Code grant flow by redirecting the end-user's user-agent to the Authorization Server's Authorization Endpoint.
		- The `OAuth2AuthorizationRequestResolver` 
			- Resolve an `OAuth2AuthorizationRequest` from the provided web request.
			- `DefaultOAuth2AuthorizationRequestResolver` - The default implementation 
				- This matches on the (default) path `/oauth2/authorization/{registrationId}`, extracting the `registrationId`
				- + Extracting the `registrationId`
				- + Using the template path above and extracted `registrationId` to build the `OAuth2AuthorizationRequest` for the associated [`ClientRegistration`](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/oauth2/client/registration/ClientRegistration.html)
		- Consider the following Spring Boot 2.x properties for an OAuth 2.0 Client registration:
			```yaml
			spring:
			  security:
			    oauth2:
			      client:
			        registration:
			          okta:
			            client-id: okta-client-id
			            client-secret: okta-client-secret
			            authorization-grant-type: authorization_code
			            redirect-uri: "{baseUrl}/authorized/okta"
			            scope: read, write
			        provider:
			          okta:
			            authorization-uri: https://dev-1234.oktapreview.com/oauth2/v1/authorize
			            token-uri: https://dev-1234.oktapreview.com/oauth2/v1/token			- 
			```
			- Given the preceding properties, a request with the base path `/oauth2/authorization/okta`(the `authorizationEndpoint` - differ from `okta.redirect-uri`) initiates the Authorization Request redirect - it means call the redirect to the `okta.oauthorization-uri `with necessity parameters has mentioned in here [Authorization Request](https://datatracker.ietf.org/doc/html/rfc6749#section-4.1.1)
			- <span style="color:#d4a216">Example demo from mine</span>: 
				- <span style="color:#555555">Setting properties configuration for OAuth2 Provider Google</span>
				  ![](https://i.imgur.com/YuSF1Yy.png)
				- <span style="color:#555555">Authorization Request To Google</span>
			     ![](https://i.imgur.com/jajDl7b.png)
				- <span style="color:#555555">Authorization Endpoint</span> 
				  (I have pre-customized this endpoint)
				  ![](https://i.imgur.com/OZ2PYGo.png)
			     ![](https://i.imgur.com/8g6qJj0.png)
				- <span style="color:#555555">Authorization-uri/Authorization Request</span> 
			     ![](https://i.imgur.com/i0bzyHE.png)
				- <span style="color:#555555">Authorization Request Parameters</span>	
			     ![](https://i.imgur.com/aCGt2Hs.png)
				- <span style="color:#555555">Authorization Response At Callback Redirect Uri</span>
			     ![](https://i.imgur.com/OhrMQnM.png)
			     ![](https://i.imgur.com/M7nSxJQ.png)
	- Customizing the Authorization Request 
		- `OAuth2AuthorizationRequestResolver` support customize the Authorization Request with additional parameters beyond the standard parameters defined in the OAuth 2.0 Authorization Framework
		- The implementation `DefaultOAuth2AuthorizationRequestResolver` support customize the Authorization Request for oauth2Login() with a <span style="color:#d4a216">***`Consumer<OAuth2AuthorizationRequest.Builder>`***</span>
		- [See example](https://docs.spring.io/spring-security/reference/servlet/oauth2/client/authorization-grants.html#_customizing_the_authorization_request)
			--- 
			<span style="color:#d4a216">Reference :</span>
			- 🍎 [Custom Authorization Request](https://www.baeldung.com/spring-security-custom-oauth-requests#authrize)			
	- Storing the Authorization Request
		- The `AuthorizationRequestRepository` - responsible for the persistence of the `OAuth2AuthorizationRequest` from the time the Authorization Request is initiated to the time the Authorization Response is received (the callback)
		- The `OAuth2AuthorizationRequest` is used to correlate and validate the Authorization Response 
			*<span style="color:#555555">for example checking the state when send request and state when comeback must be the same to avoid csrf</span>*
			![](https://i.imgur.com/BOyVBQ3.png)
		- `HttpSessionOAuth2AuthorizationRequestRepository` - The Default implementation of `AuthorizationRequestRepository` - which stores the `OAuth2AuthorizationRequest` in the `HttpSession`
		- <span style="color:#d4a216">If you have a custom implementation of `AuthorizationRequestRepository`, you can configure</span> [See example](https://docs.spring.io/spring-security/reference/servlet/oauth2/client/authorization-grants.html#_storing_the_authorization_request)
	- Requesting an Access Token
		- The default implementation of `OAuth2AccessTokenResponseClient` for the Authorization Code grant is `DefaultAuthorizationCodeTokenResponseClient` - which uses a `RestOperations` instance to exchange an authorization code for an access token at the Token Response.
			--- 
			<span style="color:#d4a216">Reference :</span>
			- 🍎 [Access Token Request](https://datatracker.ietf.org/doc/html/rfc6749#section-4.1.3)
				  ![](https://i.imgur.com/C7p4Ie7.png)
			- 🍎 [Access Token Response](https://datatracker.ietf.org/doc/html/rfc6749#section-4.1.4)
				  ![](https://i.imgur.com/pizeqot.png)
	- Customizing the Access Token Request
		- If you need to customize the pre-processing of the Token Request --> you can provide `DefaultAuthorizationCodeTokenResponseClient.setRequestEntityConverter()` with a custom `Converter<OAuth2AuthorizationCodeGrantRequest, RequestEntity<?>>`
		- The default implementation (OAuth2AuthorizationCodeGrantRequestEntityConverter) builds a `RequestEntity` - which representation representation of a standard [OAuth 2.0 Access Token Request](https://tools.ietf.org/html/rfc6749#section-4.1.3)
		- To customize only the parameters of the request --> you can provide `OAuth2AuthorizationCodeGrantRequestEntityConverter.setParametersConverter()` with a custom `Converter<OAuth2AuthorizationCodeGrantRequest, MultiValueMap<String, String>>`
		- To only add additional parameters --> you can provide `OAuth2AuthorizationCodeGrantRequestEntityConverter.addParametersConverter()` with a custom `Converter<OAuth2AuthorizationCodeGrantRequest, MultiValueMap<String, String>>`
			---
			<span style="color:#d4a216">Reference :</span>
			 - 🍎 [Custom Token Request](https://www.baeldung.com/spring-security-custom-oauth-requests#token)
	- Customizing the Access Token Response
		-   If you need to customize the post-handling of the Token Response --> you need to provide `DefaultAuthorizationCodeTokenResponseClient.setRestOperations()` with a custom configured `RestOperations`   
		- The default `RestOperations` is configured as follows: 
			```Java
			RestTemplate restTemplate = new RestTemplate(Arrays.asList(
					new FormHttpMessageConverter(),
					new OAuth2AccessTokenResponseHttpMessageConverter()));	
			restTemplate.setErrorHandler(new OAuth2ErrorResponseErrorHandler());		 
	        ```
		- `FormHttpMessageConverter` - is required since it is used when sending the OAuth 2.0 Access Token Request.
		- `OAuth2AccessTokenResponseHttpMessageConverter` - is an `HttpMessageConverter` for an OAuth 2.0 Access Token Response ---> <span style="color:#00b0f0">You can provide `OAuth2AccessTokenResponseHttpMessageConverter.setAccessTokenResponseConverter()` with a custom `Converter<Map<String, Object>, OAuth2AccessTokenResponse>` that is used for converting the OAuth 2.0 Access Token Response parameters to an `OAuth2AccessTokenResponse`</span>
		- `OAuth2ErrorResponseErrorHandler` - a `ResponseErrorHandler` that can handle an OAuth 2.0 Error, such as `400 Bad Request` +  It uses an `OAuth2ErrorHttpMessageConverter` for converting the OAuth 2.0 Error parameters to an `OAuth2Error`
		- An template to follow for customization `DefaultAuthorizationCodeTokenResponseClient` || provide your own implementation of `OAuth2AccessTokenResponseClient` [See](https://docs.spring.io/spring-security/reference/servlet/oauth2/client/authorization-grants.html#_customizing_the_access_token_response)
			--- 
			<span style="color:#d4a216">Reference :</span> 
		- 🍎 [Customize the response of token endpoint and the token claims in Spring-Authorization-Server](https://sendoh-daten.medium.com/customize-the-response-of-token-endpoint-and-the-token-claims-in-spring-authorization-server-316e52e6ea82)
		- 🍎 [Custom Token Response Handling](https://www.baeldung.com/spring-security-custom-oauth-requests#tokenRes)
2. Refresh Token
	- Refreshing an Expired Access Token Flow
	  ![](https://i.imgur.com/0Px8jmN.png)
	- Refresh token Are credentials used to obtain access tokens.  
	- Issued to the client by the authorization server and Used to obtain a new access token when the current access token becomes invalid, expires, or to obtain additional access token with identical or narrower scope.
	- A refresh token is a string representing the authorization granted to the client by the resource owner. This string is usually opaque to the client. The token denotes an identifier used to retrieve the authorization information.
	- Unlike access token - refresh tokens are intended for use only with authorization servers and are never sent to resource servers.
	- 🍎 [Refresh Token more details](https://datatracker.ietf.org/doc/html/rfc6749#section-1.5)
	---> Learn how to configure with Spring for gain OAuth2 Authorization Grants using Refresh Token: [See](https://docs.spring.io/spring-security/reference/servlet/oauth2/client/authorization-grants.html#_refreshing_an_access_token)
3. Client Credentials
	- ![](https://i.imgur.com/oXPNH3w.png) 
	- The Client can request an access token using only its client credentials when the client is requesting access to the protected resource under its control, or those of another resource owner that have been previously arranged with the authorization server
	- 🍎 [Client Credentials Grant more details](https://datatracker.ietf.org/doc/html/rfc6749#section-4.4)
	---> <span style="color:#00b050">Learn how to configure with Spring for gain OAuth2 Authorization Grants using Client Credentials</span>: [See](https://docs.spring.io/spring-security/reference/servlet/oauth2/client/authorization-grants.html#oauth2Client-client-creds-grant
4. Resource Owner Password Credentials
	- Resource Owner Password Credentials Flow
	  ![](https://i.imgur.com/kQeOufb.png)
    - The resource owner password credentials grant type is suitable in case where the resource owner has a trust relationship with the client, such as the device operating system or a highly privileged application. The authorization server should take special care when enabling this grant type and only allow it when other flows are not viable.
    - It is suitable for clients capable of obtaining the
   resource owner's credentials (username and password, typically using
   an interactive form).  
	- It is also used to migrate existing clients
   using direct authentication schemes such as HTTP Basic or Digest
   authentication to OAuth by converting the stored credentials to an
   access token
   - 🍎 [Resource Owner Password Credentials more details](https://datatracker.ietf.org/doc/html/rfc6749#section-4.3)
	---><span style="color:#00b050">Learn how to configure with Spring for gain OAuth2 Authorization Grants using Client Credentials</span>: [See](https://docs.spring.io/spring-security/reference/servlet/oauth2/client/authorization-grants.html#oauth2Client-password-grant)
5. JWT Bearer
	- The use of a JSON Web Token (JWT) Bearer Token as a means for requesting an OAuth 2.0 access token as well as for client authentication.
	- 🍎 [JWT Bearer more details](https://datatracker.ietf.org/doc/html/rfc7523#page-1)
	---><span style="color:#00b050">Learn how to configure with Spring for gain OAuth2 Authorization Grants using Client Credentials</span>: [See](https://docs.spring.io/spring-security/reference/servlet/oauth2/client/authorization-grants.html#_customizing_the_access_token_request_5)
##### Addtional Features provided by Spring Security for the OAuth2 clients
- Authorized Client Features - [See](https://docs.spring.io/spring-security/reference/servlet/oauth2/client/authorized-clients.html#oauth2Client-registered-authorized-client)
	- <span style="color:#00b050">Resolving an Authorized Client</span> -  `@RegisteredOAuth2AuthorizedClient` annotation provides the ability to resolve a method parameter to an argument value of type `OAuth2AuthorizedClient`.
	- ![](https://i.imgur.com/cUvOEC0.png)
	- <span style="color:#00b050">WebClient Integration for Servlet Environments</span> - The OAuth 2.0 Client support integrates with `WebClient` by using an `ExchangeFilterFunction`.The `ServletOAuth2AuthorizedClientExchangeFilterFunction` provides a mechanism for requesting protected resources by using an `OAuth2AuthorizedClient` and including the associated `OAuth2AccessToken` as a Bearer Token.

#### Expand
<span style="color:#00b050">Core class for building OAuth2AuthorizationRequest</span>
- ![](https://i.imgur.com/NpOmLw3.png)
<span style="color:#00b050">Core class for building OAuth2AuthorizationResponse</span> 
- ![](https://i.imgur.com/Q95NMIl.png)
- ![](https://i.imgur.com/zPxc040.png)
<span style="color:#00b050">Core class support Get User Info at UserInfoEndpoint</span>
- ![](https://i.imgur.com/YhcmNV0.png)
- ![](https://i.imgur.com/mo95cXg.png)



#### Check
- 👕 Check the state get from the Authorization Request (class `HttpSessionOAuth2AuthorizationRequestRepository`)
	- ![](https://i.imgur.com/lBjdoPs.png)
- 👕 Check authorizationRequest save in HttpSession AUTHORIZATION_REQUEST (class `HttpSessionOAuth2AuthorizationRequestRepository`)
	- ![](https://i.imgur.com/5Yh8DqP.png)
	- ![](https://i.imgur.com/LOZ20o3.png)
- 👕 Check retrieving redirect uri (class `OAuth2AuthorizationCodeGrantFilter`)
	- ![](https://i.imgur.com/Tcq46kR.png)
- 👕 Implement compare redirect uri 
	- ![](https://i.imgur.com/WDAzlmJ.png)
- 👕 Check redirect URI building, state, code from the request (class `OAuth2AuthorizationCodeGrantFilter` to `OAuth2AuthorizationResponseUtils`)
	- ![](https://i.imgur.com/mrzrTkO.png)
	- ![](https://i.imgur.com/lhwtvDr.png)
- 👕 Check if AuthorizationRequest exist after require remove a version of it self in HttpSession
	- ![](https://i.imgur.com/tsq1qNo.png)





	