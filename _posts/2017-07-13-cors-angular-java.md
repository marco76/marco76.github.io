---
title: 'Create Cross-Origin requests (CORS)'
description: 'How to allow communication between your Angular frontend and the Java Backend'
date: 2017-07-13T20:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'angular'
color: '#7AAB13'
permalink: /2017/07/13/create-cors-requests-angular-java/
categories:
  - Spring Boot
  - Angular
  - Java EE
  - REST
tags:
  - spring
  - angular
  - rest
  - java ee

image: '/assets/img/'

introduction: 'How to allow communication between your Angular frontend and the Java Backend'
---
# Create Cross-Origin HTTP requests (CORS)

During the development of your application it is a good practice to work with 2 separate server. A server for your backend and a server for your frontend.
Your JavaScript frontend will communicate with your backend to collect information using REST services.

At this moment you will incur in a problem : for security reasons browsers don’t allow that a page answering from the domainA load resources from a domainB. 
![]({{site.baseurl}}/assets/img/uploads/2017/07/13/CORS.png)

You can read the detailed explanation of the CORS mechanism here: [Mozilla](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

## How to allow CORS requests
### Angular
In our Angular request we have to add the header 
[_X-Requested-With_](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest). This header enables a webpage to update just partially.

```javascript
this.headers = new Headers({ 'Content-Type': 'application/json' })
this.headers.append('Accept', 'application/json, text/csv');
this.headers.append('X-Requested-With', 'XMLHttpRequest');
```

### Java EE
#### Filter

The filter solution can be used in Servlet compatible Servers.

``` java
@Order(Ordered.HIGHEST_PRECEDENCE)
class CorsFilter implements Filter {

    public void doFilter(ServletRequest req, ServletResponse res,
        FilterChain chain) throws IOException, ServletException {
        HttpServletResponse response = (HttpServletResponse) res;

        response.setHeader("Access-Control-Allow-Origin", "*");
        response.setHeader("Access-Control-Allow-Methods", "POST, PUT, GET, OPTIONS,  DELETE");
        response.setHeader("Access-Control-Allow-Headers", "Authorization,Content-Type, x-requested-with");
        response.setHeader("Access-Control-Max-Age", "3600");

        if (((HttpServletRequest)req).getMethod().equals("OPTIONS")) {
            response.setStatus(HttpServletResponse.SC_OK);
        } else {
            chain.doFilter(req, res);
        }
    }

    public void init(FilterConfig filterConfig) {}

    public void destroy() {}
}

```

#### Spring Boot
This solution contains Spring specific annotations.
You have to update the `ALLOWED_ORIGINS` constant with the URL of the frontend server sending the requests to the backend server. 
	
``` java
@Configuration
public class CorsConfig {

    private static final String[] ALLOWED_ORIGINS = {"http://localhost:3000",
    "http://localhost:8080"};

    @Bean
    public WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurerAdapter() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/*").allowedOrigins(ALLOWED_ORIGINS);
            }
          };
       }
    }
```
