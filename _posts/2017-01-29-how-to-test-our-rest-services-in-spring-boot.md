---
id: 810
title: How to test our REST services using Spring Boot?
date: 2017-01-29T10:47:06+00:00
author: Marco Molteni
layout: post
guid: http://javaee.ch/?p=810
permalink: /2017/01/29/how-to-test-our-rest-services-in-spring-boot/
dsq_thread_id:
  - "5565921873"
categories:
  - Angular
  - java
  - REST
  - Spring
  - Spring Boot
  - Uncategorized
tags:
  - Angular
  - java
  - junit
  - mock
  - mockito
  - REST
  - spring boot
  - tests
---
Spring Boot offers us easy to use features to test our REST service. Our goal is to check that our REST Controller answers correctly to an HTTP request.

Here we are not interested to test the services layer or the database. For this reason we have to mock everything is around the controller.

<img class="alignnone size-full wp-image-809" src="https://i1.wp.com/javaee.ch/wp-content/uploads/2017/01/Test.png?resize=400%2C305" data-recalc-dims="1" />

### Code sample

You can find the working code for the REST controller here:

<https://github.com/marco76/SpringAngular2TypeScript/blob/master/server/src/test/java/ch/javaee/demo/angular2/web/DavisControllerTest.java>

### Mock the request

Spring boot gives us the possibility to simulate the http request without any browser using the annotation [@WebMvcTest](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/api/org/springframework/boot/test/autoconfigure/web/servlet/WebMvcTest.html) . This annotation configure [@MockMvc](http://docs.spring.io/spring-framework/docs/5.0.0.BUILD-SNAPSHOT/javadoc-api/org/springframework/test/web/servlet/MockMvc.html?is-external=true) that simulates the request for the tests.

### Mock the service

As we see in the picture we don&#8217;t need to test the service, maybe it is retrieving some data from the database. We can use the annotations [@MockBean](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/api/org/springframework/boot/test/mock/mockito/MockBean.html) or [@SpyBean](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/api/org/springframework/boot/test/mock/mockito/SpyBean.html).

`@MockBean` creates a mock of the entire object.
  
`@SpyBean` allows to mock only some methods of an object.

### To the code

Import the required classes:

    @RunWith(SpringRunner.class)
    @WebMvcTest(DavisController.class)
    public class DavisControllerTest {
    

In our case we have specified the controller to test in the annotation.

    @SpyBean
    private DavisService davisService;
    
    @Autowired
    private MockMvc mvc;
    

We use `@SpyBean` because we are interested in mocking only the method that access the database.

    @Test
    public void resultListTest() throws Exception {
    
    String expectedResult = "[{\"year\":2015,\"winner\":\"Marco Team\"}]";
    
    given(this.davisService.getResultList()).willReturn(mockedDavisData());
    
    this.mvc.perform(get("/result_list"))
       .andExpect(status().isOk())
       .andExpect(content().json(expectedResult));
    }
    

In our test we define an `expectedResult`, part of the JSON returned as answer.

To mock the &#8216;database method&#8217; `getResultList()` we use the features of mockito that is imported with our spring boot pom:

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>
            spring-boot-starter-test
        </artifactId>
    </dependency>
    

The method call `this.mvc.perform(get("/result_list"))` is a static method that mock the GET request. It builds an HttpServletRequest behind the scenes and wait for the Http answer of the controller.

At this point we can test the result of the controller with the JSON string defined as expected result.

`andExpect` is part of the interface [ResultActions](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/test/web/servlet/ResultActions.html).