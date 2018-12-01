---
title: 'Config files in Java EE / Jakarta EE using MicroProfile'
description: ''
date: 2018-11-14T10:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'javaee'
color: '#7AAB13'
permalink: /javaee-microprofile-config-file/
categories:
  - Java EE
tags:
  - Java EE
 
image: '/assets/img/'

introduction: 'How to store the configuration in external files?'
---

## The Spring way
The developers that work with Spring know very well how easy is to separate the configuration of the application from the code.
In Spring there is an `application.properties` file in the resources that contains the main configuration of the application. The properties can be used in the code injecting fields with the `@Value` annotation or, better, injecting a properties class with `@ConfigurationProperties` (only SpringBoot).

You can find more details for Spring here: [24. Externalized Configuration](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html)

## Java EE before MicroProfile

In Java EE the procedure was more complicated. To store properties in a file the solutions were the following: [StackOverflow - Where to place and how to read configuration resource files in servlet based application?](https://stackoverflow.com/questions/2161054/where-to-place-and-how-to-read-configuration-resource-files-in-servlet-based-app)

## Java EE with MicroProfile ConfigFeature

MicroProfile introduces the [`Config Feature`](https://github.com/eclipse/microprofile-config). Thanks to this project Java EE applications can use external configuration files with ease.

### Tutorial with Java EE 8 (Payara)

For our example we use [Payara](https://www.payara.fish/) but you can use other Java EE servers.

*pom.xml*
```xml
<dependencies>
    <dependency>
        <groupId>javax</groupId>
        <artifactId>javaee-api</artifactId>
        <version>8.0</version>
        <scope>provided</scope>
    </dependency>
    
        <!-- MicroProfile Config 1.3 API -->

    <dependency>
        <groupId>org.eclipse.microprofile.config</groupId>
        <artifactId>microprofile-config-api</artifactId>
        <version>1.3</version>
         <scope>provided</scope>
    </dependency>
</dependencies>
```

In our pom we have only 2 dependencies: Java EE 8 and MicroProfile Config API. The implementations of the these dependencies are already integrated in Payara.

*sources structure*

```bash
./src
├── main
│   ├── java
│   │   └── io
│   │       └── javademo
│   │           └── micro
│   │               ├── ApplicationConfig.java
│   │               └── api
│   │                   └── ExampleResource.java
│   └── resources
│       └── META-INF
│           ├── beans.xml
│           └── microprofile-config.properties

```
*beans.xml*

It's important to don't forget the `beans.xml` file in WEB-INF or META-INF. The presence of this empty file enables CDI.
Without it `@Inject` will not work.

*microprofile-config.properties*

This file contains the properties that we want to inject. In our case we add simple:
```
name=marco
```

*ApplicationConfig.java*

This is the entry point of the REST services:
```java
import javax.ws.rs.ApplicationPath;
import javax.ws.rs.core.Application;

@ApplicationPath("api")
public class ApplicationConfig extends Application {
}
```

*ExampleResource.java*

This file define the REST endpoint that returns the name defined in the properties file:

``` java
package io.javademo.micro.api;

import org.eclipse.microprofile.config.inject.ConfigProperty;

import javax.inject.Inject;
import javax.ws.rs.GET;
import javax.ws.rs.Path;

// with @Path we define the url of the endpoint:
// http://javademo.io/api/example
@Path("example")
public class ExampleResource {

    // inject is required to import the property
    // to work beans.xml has to be present in webapp/WEB-INF
    @Inject
    // the property name has to match the propety in 
    // META-INF/microprofile-config.properties
    @ConfigProperty(name="name", defaultValue = "(Name not present)")
    private String name;

    @Inject
    @ConfigProperty(name="country", defaultValue = "(Country not present)")
    private String country;

    @GET
    public String hello() {
        return String.format("Name: %s, Country: %s", name, country);
    }
}
```

To inject the field we use the CDI annotation `@Inject`.
To define which property is injected we @ConfigProperty that comes with MicroProfile:

```java
@ConfigProperty(name="name", defaultValue = "(Name not present)")
private String name;
```

If case the property is not found in the properties file, the defaultValue is used to assign the field.

*Use of Config*

The same result can be achieved injecting the [Config](https://github.com/eclipse/microprofile-config/blob/master/api/src/main/java/org/eclipse/microprofile/config/Config.java) object:

```java
@Inject
private Config config;

config.getValue("name", String.class);
```

*REST answer*

The result:
``` bash
Name: marco, Country: (Country not present)
```

