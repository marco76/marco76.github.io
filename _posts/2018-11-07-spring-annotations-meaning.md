---
title: 'Spring common annotations meaning'
description: 'What is this annotation doing in the code?'
date: 2018-11-06T10:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'spring'
color: '#7AAB13'
permalink: /spring-annotation-meaning/
categories:
  - Spring
tags:
  - spring
 
image: '/assets/img/'

introduction: 'What is this annotation doing in the code?'
---

During the review of a recent project I listed the Spring annotations used during the development.

Good Java developers without Spring experience could have difficulties with all this annotations.
For this reason the list want to help them to quickly understand what's the goal of the annotation and a link to the documentation.

Some are commonly used in every project, a few are less known. I will add some examples, for the moment you can find the link to the documentation.

# Spring annotations

[*@Autowired*](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Autowired.html)

This is one of the must (mis)used annotation of Spring for injecting Beans.
In our team we decided to don't allow the use of this annotation in our code with the exception of the Test classes.
We injected beans using the constructor as recommended by Spring.
Here the explanation about our decision: [Spring best practices](http://marco.dev/spring-boot-best-practices/) 

[*@Bean*](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-java-basic-concepts)

Method annotation, the returned value is registered as a bean within a BeanFactory. A Beans is (by default) a single instance of an class.
In our project we used a lot of @Bean instances to configure the connection to external systems.

[*@Component*](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/stereotype/Component.html)

Spring auto-detected generic component. In our project we used only one time. We prefer to use specialized components (Controller, Repository) 

[*@ConditionalOnProperty*](https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/autoconfigure/condition/ConditionalOnProperty.html)

This annotation is helpful when different beans have to deployed in different environments (integration tests / local development / production). Using a property file we can enable or disable the instantiation of the beans at the startup. I liked this annotation a lot.

[*@Configuration*](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-java-basic-concepts)

Indicates that a class declare one or more @Bean methods.
At the start of the application Spring reads the @Configuration classes and registers the declared beans.

[@ConfigurationProperties](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html#boot-features-external-config-vs-value)

With @ConfigurationProperties we replaced long lists of @Value fields in configuration files with only one import of a property object.
This annotation is helpful when you have to work with a lot of external parameters (e.g. property files)

[@ComponentScan](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/ComponentScan.html)

This annotation tells to Spring that some packages have to be scanned searching for annotated beans. Without this annotation your @Bean, @Controller etc. don't have any effect.

[@EnableConfigurationProperties](https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/context/properties/EnableConfigurationProperties.html)

It allows the scan of the @ConfigurationProperties annotation.

[*@Repository*](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/stereotype/Repository.html)

It is a specialization of @Component (autodetected during the startup scanning). The Repository class contains methods to store and retrieve data in a collection of objects (e.g. database).
It represents the DAO in Java EE.

[*@RestController*](https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html#mvc-controller)

It's an annotation that includes @Controller and @ResponseBody and it is used to declare web controller that maps http requests with Spring functions.

[*@RequestMapping*](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/bind/annotation/RequestMapping.html)

It maps a path in the URL request with a method declared in Spring.
In the project we used the specializations [@GetMapping](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/bind/annotation/GetMapping.html), [@PostMapping](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/bind/annotation/PostMapping.html) etc.

## Coming soon:

@Service
@Value
@Scheduled
## Spring integration

@EnableIntegration
@IntegrationComponentScan
@EnableIntegrationManagement
@EnableIntegrationMBeanExport
@Splitter

## Tests
@ActiveProfiles
@ContextConfiguration
@MockBean
@Qualifier
@TestPropertySource