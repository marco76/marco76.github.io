---
title: 'Spring Boot: REST controller Test example'
redirect_to:
- https://www.ngjava.com/spring-boot-test-controller/
description: 'How to test the @RestController'
date: 2017-10-01T01:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'spring'
color: '#7AAB13'
permalink: /2017/10/01/spring-boot-test-controller/
categories:
  - Java
  - REST
  - Spring
tags:
  - java
  - spring
 
image: '/assets/img/'

introduction: 'How to test the @RestController with Spring'
---

## How it works

The annotation `@WebMvcTest` configure only the components that usually interest the web development.
 
<img src="{{site.baseurl}}/assets/img/uploads/2017/10/spring_test_annotation_1.png"/>

As shown in the image `@Service` and `@Repository`are not configured.

When we call the `@Service` from the `@Controller` we return the mocked object.

<img src="{{site.baseurl}}/assets/img/uploads/2017/10/spring-mock.png"/>

# Controller example

This is a very simple controller that calls a service and returns a custom object containing a text value:

``` java

@RestController
public class SimpleController {

    private SimpleService simpleService;

    public SimpleController(SimpleService simpleService) {
        this.simpleService = simpleService;
    }

    @GetMapping(value = "/simple",produces = MediaType.APPLICATION_JSON_VALUE)
    public ResponseEntity<StringJsonObject> simpleResult() {
        return ResponseEntity.ok(simpleService.getText());
    }
}
```

Here the service code:

``` java

@Service
public class SimpleServiceImpl implements SimpleService{
    @Override
    public StringJsonObject getText(){
        return new StringJsonObject("Cool!");
    }
}
```

The returned object:

``` java
public class StringJsonObject {

    private String content;

    public StringJsonObject(String content) {
        this.content = content;
    }

    public String getContent() {
        return content;
    }
}
```

## The test with comments

Here the code used to test the controller:

``` java

// SpringRunner is an alias of SpringJUnit4ClassRunner
// it's a Spring extension of JUnit that handles the TestContext
@RunWith(SpringRunner.class)

// we test only the SimpleController
@WebMvcTest(SimpleController.class)

public class SimpleControllerTest {

    // we inject the server side Spring MVC test support
    @Autowired
    private MockMvc mockMvc;

    // we mock the service, here we test only the controller
    // @MockBean is a Spring annotation that depends on mockito framework
    @MockBean
    private SimpleService simpleServiceMocked;

    @Test
    public void simpleResult() throws Exception {

        // this is the expected JSON answer
        String responseBody = "{\"content\":\"Hello World from Spring!\"}";

        // we set the result of the mocked service
        given(simpleServiceMocked.getText())
                .willReturn(new StringJsonObject("Hello World from Spring!"));

        // the test is executed:
        // perform: it executes the request and returns a ResultActions object
        // accept: type of media accepted as response
        // andExpect: ResultMatcher object that defines some expectations
        this.mockMvc.perform(get("/simple")
                .accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk())
                .andExpect(content().string(responseBody));
    }
}

```

