---
title: 'REST and Spring Boot: manage users rights with a custom Filter and properties'
description: 'Simple solution when only few users have writing rights.'
date: 2017-07-08T23:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'spring'
color: '#7AAB13'
permalink: /2017/07/09/rest-spring-boot-user-authorization-with-properties/
redirect_to:
  - https://www.ngjava.com/rest-spring-boot-user-authorization-with-properties/
categories:
  - Spring Boot
  - Security
  - Java
  - REST
tags:
  - spring
  - security
  - rest

image: '/assets/img/'

introduction: 'Simple solution when only few users have writing rights.'
---
This is a real world use case to be used only with similar restrictions.
We had a lot of constraint in our development (see the requirements) but we had to implement an effettive solution to limit the user access to the REST resources on the backend.

## Requirements / Constraints

- we cannot modify the database structure ergo we cannot create a table with roles (best solution in this case) 
- the authentication is done by an external service (using OAuth2) that cannot be modified
- all the authenticated users are allowed to read the data
- only few selected users are allowed to update the data
- the application is already in production and is developped using Spring Boot

## Implementation

 Database to store the user information and with only the information retrieved from the authentication provider (username and token), we decided to store the usernames of the authorized users in the properties loaded with the application (config file or enviornment if defined).

In Spring we created a new filter that throw an error if a non authorized user try to call a REST method that (according to the convention) should update the data.

### The configuration

We store the usernames and the filtered methods in the configuration (file, server).
This allow us to update the users list without rebuilding our application:

In your application.properties:

``` properties

#To grant the rights to all users use the character: *
security.users.right.write=${SERVER_PARAM_USERS_WRITE:marco,tom,jerry}
security.methods.secured=POST,PUT,DELETE

```

```java

// methods to be secured, typically 'POST', 'DELETE', 'PUT'
@Value("#{'${security.methods.secured}'.split(',')}")
private List<String> securedMethodList;

// usernames of the allowed users
@Value("#{'${security.users.right.write}'.split(',')}")
private List<String> userList;

```

We allow to 'disable' the security feature with a special key to use in the configuration:

```java

// special char to grant access to all the users
private static final String ALLOW_ALL_USERS="*";

```

### The filter implementation ###

When the filter is called at each HttpRequest the method doFilter is called.
In this method we have to verify if user is trying to access a secured resource and, in this case, if his username is in the list of the authorized users.

```java

public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {

        // we get the authenticated user from the context
        Authentication authentication = SecurityContextHolder.getContext().getAuthentication();

        // if the method is not secured (e.g. 'GET') or all the users are allowed to access
        // we skip the validation
        if (securedMethodList.contains(((HttpServletRequest)request).getMethod())
            && !userList.get(0).equals(ALLOW_ALL_USERS)){

            if (authentication.getPrincipal() != null) {
                // the username is the 'principal' some implementations are contains the UserDetails
                if (!userList.contains(authentication.getPrincipal().toString())) {
                    // if is not in the list of allowed users we return a response with status 403
                    throwAccessError((HttpServletResponse) response, HttpServletResponse.SC_FORBIDDEN);
                    return;
                }
            } else {
                // the user is not authenticated, we return a response with status 401
                throwAccessError((HttpServletResponse) response, HttpServletResponse.SC_UNAUTHORIZED);
                return;
            }
        }
        // the user is allowed to access the ressource
        chain.doFilter(request, response);
    }

```

## Tests ###

Spring is very helpful when it comes to tests. It comes with mocks for every component of our code.

``` java

@RunWith(SpringRunner.class)
@SpringBootTest(classes = {WriteAccessRightFilter.class})
public class SecurityTest {

    @Autowired
    private WriteAccessRightFilter writeAccessRightFilter;

    private MockFilterChain mockFilterChain;
    private MockHttpServletRequest mockHttpServletRequest;
    private MockHttpServletResponse mockHttpServletResponse;

    @Value("#{'${security.users.right.write}'.split(',')}")
    private List<String> usersAllowed;


    @Before
    public void setUp() {

        mockFilterChain = new MockFilterChain();
        mockHttpServletRequest = new MockHttpServletRequest();
        mockHttpServletResponse = new MockHttpServletResponse();
        // we redefine the authorized users
        // it allows us to test the special char '*'
        writeAccessRightFilter.setUserList(Arrays.asList("marco","ToM"));
    }

    @Test
    @WithMockOAuth2User(username = "marco")
    public void userWithWritingRightsTest() throws IOException, ServletException {
        
        // the request is of type 'POST' -> it's secured
        mockHttpServletRequest.setMethod("POST");
        
        // with doFilter we call our Filter that is in the chain
        writeAccessRightFilter.doFilter(mockHttpServletRequest, mockHttpServletResponse, mockFilterChain);

       // we verify that our Filter returned 200 (ok) 
       assertEquals(200, mockHttpServletResponse.getStatus());
    }


    @Test
    @WithMockOAuth2User(username = "DefaultUser")
    public void userWithoutWritingRightsTest() throws IOException, ServletException {
        mockHttpServletRequest.setMethod("POST");
      
        writeAccessRightFilter.doFilter(mockHttpServletRequest, mockHttpServletResponse, mockFilterChain);

        // the filter should return an error (403) if the user is not allowed to call a POST resource
        assertEquals(403, mockHttpServletResponse.getStatus());
    }

     /**
    We test that a user without writing rights has access to the GET resource 
    **/

    @Test
    @WithMockOAuth2User(username = "DefaultUser")
    public void userWithoutWritingRightsCanReadTest() throws IOException, ServletException {
        mockHttpServletRequest.setMethod("GET");
       
        writeAccessRightFilter.doFilter(mockHttpServletRequest, mockHttpServletResponse, mockFilterChain);

        assertEquals(200, mockHttpServletResponse.getStatus());
    }

    /**
    We test that using '*' every authenticated user can access to the secured REST resources
    **/

    @Test
    @WithMockOAuth2User(username = "DefaultUser")
    public void allUsersAreAllowedToWriteTest() throws IOException, ServletException {
        // we give writing rights to everybody authenticated
        writeAccessRightFilter.setUserList(Arrays.asList("*"));
        mockHttpServletRequest.setMethod("DELETE");

        writeAccessRightFilter.doFilter(mockHttpServletRequest, mockHttpServletResponse, mockFilterChain);

        assertEquals(200, mockHttpServletResponse.getStatus());
    }
}

```

Because we don't use the default UsernamePasswordAuthenticationToken we have to define a custom @WithMockOAuth2User similar to [@WithMockUser](http://docs.spring.io/spring-security/site/docs/current/reference/html/test-method.html#test-method-withmockuser) :

``` java

public class WithMockOAuth2SecurityContextFactory implements WithSecurityContextFactory<WithMockOAuth2User>{

    @Override
    public SecurityContext createSecurityContext(WithMockOAuth2User user) {
        SecurityContext context = SecurityContextHolder.createEmptyContext();

        List<GrantedAuthority> authorities = AuthorityUtils.createAuthorityList("None");
        UsernamePasswordAuthenticationToken authenticationToken = new UsernamePasswordAuthenticationToken(user.username(), null, authorities);

        Authentication authentication = new OAuth2Authentication(getOauth2Request(), authenticationToken);
        context.setAuthentication(authentication);
        return context;
    }

    private OAuth2Request getOauth2Request () {
        String clientId = "oauth-client-id";
        Map<String, String> requestParameters = Collections.emptyMap();
        boolean approved = true;
        String redirectUrl = "http://test.ch";
        Set<String> responseTypes = Collections.emptySet();
        Set<String> scopes = Collections.emptySet();
        Set<String> resourceIds = Collections.emptySet();
        Map<String, Serializable> extensionProperties = Collections.emptyMap();
        List<GrantedAuthority> authorities = AuthorityUtils.createAuthorityList("None");

        OAuth2Request oAuth2Request = new OAuth2Request(requestParameters, clientId, authorities,
                approved, scopes, resourceIds, redirectUrl, responseTypes, extensionProperties);

        return oAuth2Request;
    }
}

```

``` java

@Retention(RetentionPolicy.RUNTIME)
@WithSecurityContext(factory = WithMockOAuth2SecurityContextFactory.class)
public @interface WithMockOAuth2User {

    String username() default "muster";
}

```