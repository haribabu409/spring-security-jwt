Spring Security + JWT Authorization

Authentication : Hardcode user authentication done with UserDetailsService
JWT : Using a hardcoded secret.
/authenticate method is used for authentication, which gives back jwt. For all other requests, Bearer JWT can be used as value for Authorization

********************
Step1 - Create JWT
********************
Add dependencies like security, Web, jjwt and Jaxb-api (Needed for java post 9)

Api1: /hello end point. Prints you Hello world message. This api is authenticated.

WebSecurityConfigurerAdapter : configure(AuthenticationManagerBuilder)
auth.userDetailsService(userdetailsService);

UserDetailsService : loadUserByUserName(userName)
Hardcoded user name and password is configured. 

JwtUtil - Util class for JWT
createToken, validateToken, secret_Key.

Api2: /authenticate api is used for authentication. Takes userName and password as inputs and returns JWT. 
AuthenticationRequest
AuthenticationResponse

Now this api cannot be called with out completing authentication. 
WebSecurityConfigurerAdapter : configure(HttpSecurity)
.antMatchers(“/authenticate”).permitAll()

********************
Step2 - Intercept incoming requests
********************

Create a filter class that extends OncePerRequestFilter
doFilterInternal(HttpServletRequest, HttpServletResponse, FilterChain)
Get jwt and validate. If valid, set token on to securityContextHolder authentication.

And inject this created filter before UserPasswordAuthenticationFilter.
And make sessionManagement on http as stateless. (not to create sessions)
