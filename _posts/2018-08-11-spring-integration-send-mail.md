---
title: 'Spring Integration mail'
description: ''
date: 2018-08-11T10:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'spring'
color: '#7AAB13'
permalink: /2018/08/11/spring-integration-send-mail/
categories:
  - Spring
tags:
  - spring
 
image: '/assets/img/'

introduction: 'How to send email using Spring Integration and create smtp unit tests'
---

# How to send email using Spring Integration and create smtp unit tests

I'm working more and more often with Spring Integration and I find it great. The only problem is that most of the basic examples are based on the XML configuration and my projects are more annotation/dsl oriented.

This is the first of a series of demos about some use cases of Spring Integration. The examples will be integrated in the javademo.io website.

As usual the goal of these articles is to remember how to implement the solution in the future (it's kind of funny to look for a solution on the web and find your own article written 4 or 5 year before).

For an extensive documentation you can look in the spring.io website/source code.

## Where is the code

[https://github.com/marco76/spring-integration-demo](https://github.com/marco76/spring-integration-demo)

To run the code you can start the unit tests, they simulate a smtp server and they submit some MailMessages to Spring Integration.

Annotations: [EmailTest.java](https://github.com/marco76/spring-integration-demo/blob/master/src/test/java/ch/javaee/springdemo/mail/EmailTest.java)

DSL: [EmailDslTest.java](https://github.com/marco76/spring-integration-demo/blob/master/src/test/java/ch/javaee/springdemo/mail/EmailDslTest.java):

## Send a mail using annotations


We define a JavaMailSender bean in the configuration that connect to the mailserver.

``` java

@Configuration
@PropertySource("classpath:/smtp.properties")
public class MailConfig {

    @Value("${smtp.host}")
    private String smptHost;

    @Value("${smtp.port}")
    private Integer smtpPort;

    @Bean
    public JavaMailSenderImpl mailSender() {
        JavaMailSenderImpl mailSender = new JavaMailSenderImpl();

        mailSender.setHost(smptHost);
        mailSender.setPort(smtpPort);

        return mailSender;
    }

```

We create the channel that will receive an email and we use a @ServiceActivator to send the email to the smtp.

[MailConfig.java](https://github.com/marco76/spring-integration-demo/blob/master/src/main/java/ch/javaee/springdemo/integration/MailConfig.java)

``` java

@Bean
public MessageChannel smtpChannel() {
    return new DirectChannel();
}

@ServiceActivator(inputChannel = "smtpChannel", outputChannel = "nullChannel")
public MessageHandler mailsSenderMessagingHandler (Message<MailMessage> message) {

    MailSendingMessageHandler mailSendingMessageHandler = 
    new MailSendingMessageHandler(mailSender());
    
    mailSendingMessageHandler.handleMessage(message);

    return mailSendingMessageHandler;
}

```
We declare explicitly a channel (smtpChannel) that receives the messages to send. This is optional, if you don't create the bean Spring will do it for you.

We use a [MailSendingMessageHandler](https://docs.spring.io/spring-integration/docs/current/api/org/springframework/integration/mail/MailSendingMessageHandler.html) to send the message.

As you can see it does a lot of magic, it receives a Message, if the payload of the message is a MailMessage this one is passed to the smtp server.

You can see how the class is used in the test class [EmailTest.java](https://github.com/marco76/spring-integration-demo/blob/master/src/test/java/ch/javaee/springdemo/mail/EmailTest.java)

``` java

@RunWith(SpringRunner.class)
@ContextConfiguration(classes = {IntegrationConfig.class, MailConfig.class})
public class EmailTest {

    @Autowired
    MessageChannel smtpChannel;

    @Test
    public void mailsend() throws IOException {

        SimpleSmtpServer mailServer = SimpleSmtpServer.start(12345);
        
        smtpChannel.send(new GenericMessage<>(buildMailMessage("Test 1", "content 1")));
        smtpChannel.send(new GenericMessage<>(buildMailMessage("Test 2", "content 2")));
        
        mailServer.stop();

        List<SmtpMessage> messagesSent = mailServer.getReceivedEmails();

        assertEquals(2, messagesSent.size());
    }

    private MailMessage buildMailMessage(String subject, String content){

        MailMessage mailMessage = new SimpleMailMessage();
        mailMessage.setTo("marco.molteni@j.ch");
        mailMessage.setFrom("marco@j.ch");
        mailMessage.setText(content);
        mailMessage.setSubject(subject);

        return mailMessage;
    }
}
```

To simulate the server we use ```com.dumbster.smtp.SimpleSmtpServer;```, the library simulates a local mail server and records the messages sent.

To send the mail message in the channel it's enough to send a MailMessage in an integration Message. Spring automatically read the payload of the integration's message and send it.

## Send a message using DSL
If you want to use some code that is better suited for your lambda expressions, you can use Spring Integration DSL. Since the version 5.0 of Spring integration the DSL is integrated in Integration Core.

[MailDslConfig.java](https://github.com/marco76/spring-integration-demo/blob/master/src/main/java/ch/javaee/springdemo/integration/MailDslConfig.java)

``` java
@Bean
public IntegrationFlow smtpFlow() {
    return IntegrationFlows.from("smtpFlowChannel")
        .handle(new MailSendingMessageHandler(mailSender()))
        .get();
}
```

The ```smtpFlowChannel``` is created directly by Spring because there are no channels configured with this name.
The message received is sent to the Mail handler.

To use this code [EmailDslTest.java](https://github.com/marco76/spring-integration-demo/blob/master/src/test/java/ch/javaee/springdemo/mail/EmailDslTest.java):
``` java

 @Autowired
    MessageChannel smtpFlowChannel;

    @Test
    public void mailsend() throws IOException {

        SimpleSmtpServer mailServer = SimpleSmtpServer.start(12345);
        smtpFlowChannel.send(new GenericMessage<>(buildMailMessage("Test 2", "content 1")));

```

## Spring configuration
 
 Don't forget to declare that your project is using Spring integration or Spring won't find your interfaces.
 
 ``` java
 @EnableIntegration
 @IntegrationComponentScan
 public class IntegrationConfig {
 }
 ```

## Maven

Dumbster as fake smtp:

 ``` xml 
 <dependency>
    <groupId>com.github.kirviq</groupId>
    <artifactId>dumbster</artifactId>
    <version>1.7.1</version>
    <scope>test</scope>
 </dependency>
 ```
 
 JavaMail:
 
 ``` xml 
 <dependency>
    <groupId>javax.mail</groupId>
    <artifactId>mail</artifactId>
    <version>1.4</version>
 </dependency>
 ```
 
 Spring dependencies:
 
 ``` xml 
 <dependency>
    <groupId>org.springframework.integration</groupId>
    <artifactId>spring-integration-mail</artifactId>
 </dependency>
```

## What's next
 
Everything is ready for more complicated e-mails in HTML and with attachments. If you are interested drop me a mail or open an issue on GitHub.