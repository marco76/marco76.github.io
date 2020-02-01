---
id: 368
title: Spring Integration Hello World tutorial
date: 2013-04-20T19:34:44+00:00
author: Marco Molteni
layout: post
guid: http://marco.dev/?p=368
permalink: /2013/04/20/spring-integration-hello-world-tutorial/
video_type:
  - '#NONE#'
categories:
  - Intellij
  - java
  - Java 6
  - Maven
  - Spring
---
Here you can find another Hello World example that shows the first steps to build a Spring Integration application.

In our example we create a new Java application that send a message &#8216;World&#8217; to a channel, a Spring service receives and modifies this string (&#8216;Hello World!&#8217;). Eventually and the message is sent to a print service through a second channel.

[<img class="alignnone size-medium wp-image-369" alt="spring_integration_1" src="{{site.baseurl}}/assets/img/uploads/2013/04/spring_integration_1.png?resize=300%2C145" data-recalc-dims="1" />]({{site.baseurl}}/assets/img/uploads/2013/04/spring_integration_1.png)

The structure of the application is very simple:

[<img class="alignnone size-medium wp-image-373" alt="spring_integration_2" src="{{site.baseurl}}/assets/img/uploads/2013/04/spring_integration_2.png?resize=300%2C224" data-recalc-dims="1" />]({{site.baseurl}}/assets/img/uploads/2013/04/spring_integration_2.png)

You can find all the code on gitHub: <a title="https://github.com/marco76/SpringIntegrationExamples" href="https://github.com/marco76/SpringIntegrationExamples" target="_blank">https://github.com/marco76/SpringIntegrationExamples</a>

The project is build with maven (and developed using IntelliJ). In the pom.xml you have to import the usual libraries and Spring Integration:

```xml
<dependency>
    <groupId>org.springframework.integration</groupId>
    <artifactId>spring-integration-core</artifactId>
    <version>2.2.3.RELEASE</version>
</dependency>
```

The _spring-config.xml_ configure the channels used by the application:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:integration="http://www.springframework.org/schema/integration"
       xmlns:beans="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/integration http://www.springframework.org/schema/integration/spring-integration.xsd">

    <context:annotation-config />

    <context:component-scan base-package="ch.javaee.integration.example.helloWorld"/>

    <!-- this channel is called by the application and the message is passed to it -->
    <integration:channel id="inputChannel"/>

    <!-- this channel receive the modified message -->
    <integration:channel id="outputChannel"/>

    <!-- this service transform the message in input-channel and send the result to output-channel -->
    <!-- the service method to call is referenced in explicitly -->
    <integration:service-activator input-channel="inputChannel" ref="helloService" method="sayHello" output-channel="outputChannel"/>

    <!-- this service receives a message and pass it to printerService -->
    <!-- the method that consumes the message is implicitly defined by the @ServiceActivator annotation or it should be the only
    method in the class -->
    <integration:service-activator input-channel="outputChannel" ref="printerService"/>

</beans>
```

We have 2 channels that are used to transfer messages between services. The _service-activator_ wait for messages sent to the _input-channel_ it call a predefined service (ex. _helloService_). If the called service returns a value this value is sent to the _output-channel_.

```java
/**
 * The goal of this example is to show how a message can be sent to one input channel,
 * be transformed by a service, sent to a second channel and consumed by a second service
 */
public class HelloApp {
    public static void main(String[] args) {

        // load the Spring context
        ApplicationContext context = new ClassPathXmlApplicationContext("spring-config.xml");

        // get the reference to the message channel
        MessageChannel channel = context.getBean("inputChannel", MessageChannel.class);

        // create a message with the content "World"
        Message<String> message = MessageBuilder.withPayload("World").build();

        // send the message to the inputChannel
        channel.send(message);
    }
}
```

The main class of the application (_HelloApp_) load the spring beans and send a message to the _inputChannel_.
  
The _service-activator_ read the message and pass it to method _sayHello_ _helloService_:

```xml
<integration:channel id="inputChannel"/>
<integration:service-activator input-channel="inputChannel" ref="helloService" method="sayHello" output-channel="outputChannel"/>
```


```java
/**
 * This service simply modify the original message and return the new message
 */
@Component
public class HelloService {

    public String sayHello(String name) {
        return "Hello " + name + "!";
    }
}
```

HelloService is a simple bean Spring (_@Component_), it doesn&#8217;t know anything about Spring integration.
  
The method _sayHello_ return a String value (&#8220;Hello &#8221; + &#8220;world&#8221; + &#8220;!&#8221;). Spring send this string to the _output-channel_ defined in the configuration.

```xml
<integration:channel id="outputChannel"/>
<integration:service-activator input-channel="outputChannel" ref="printerService"/>
```
In this case we don&#8217;t need to define which method will receive the message. Spring doesn&#8217;t need to explicitly know the name if:
  
&#8211; there is only one method in the class
  
&#8211; the method is annotated with @ServiceActivator (as in our case)

```java
/**
 * This service consume the message and print it on the console
 */

@Component
public class PrinterService {

    // if only one method is present in the class the @ServiceActivator is not necessary
    // in alternative the method has to be explicitly declared in the configuration

    @ServiceActivator
    public void printValue(String value){
        System.out.println(value);
    }
}
```

The PrinterService receive the message from _outputChannel_ and print it (&#8220;Hello World!&#8221;) to the console. The method should not return any value to avoid the following error:
  
_Exception in thread &#8220;main&#8221; org.springframework.integration.support.channel.ChannelResolutionException: no output-channel or replyChannel header available_