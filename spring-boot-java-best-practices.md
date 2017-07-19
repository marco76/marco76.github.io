---
title: Spring Boot best practices
permalink: java-spring-boot-best-practices/
layout: post

introduction: 'Some tips for your Spring development'
---

_Work in progress, this post is still in 'work in progress' status_

## Injection
### Avoid field and setter injection
It should be used only for optional dependencies or in test code.

_Good_
- easy to use

_Bad_
- the bean can be changed runtime with a new call of the setter;
- the field can reference a null instance, you should add the annotation @Required to enforce the dependency;
- it's easy to add dependencies, the class risk to violate the single responsibility principle becoming a container of services.

References
- Olivier Gierke: [why-field-injection-is-evil](http://olivergierke.de/2013/11/why-field-injection-is-evil/)

### Use constructor injection

_Good_
- the beans cannot be null;
- the object is immutable;
- the object can be defined final;
- in case the bean has only one constructor you can omit [@Autowired](http://docs.spring.io/spring-framework/docs/current/spring-framework-reference/htmlsingle/#beans-autowired-annotation
);
- force to better think the responsibility of the class. 

References
- Spring Blog: [How not to hate spring in 2016](https://spring.io/blog/2015/11/29/how-not-to-hate-spring-in-2016)
- Spring Documentation : [Reference](http://docs.spring.io/spring-framework/docs/current/spring-framework-reference/htmlsingle/#beans-constructor-injection)

### Use @Inject in place of @Autowired
- @Inject is compatible with other containers
- @Inject allows the declaration of injection points. 