---
id: 1104
title: How to solve the Angular redirection error (404) using Java
date: 2017-04-12T18:23:29+00:00
author: Marco Molteni
layout: post
main-class: 'javaee'
color: '#B31917'
permalink: /2017/04/12/how-to-solve-the-redirection-404-error-in-angular-using-java/
image: /wp-content/uploads/2017/03/logo_ang-100x39.png
categories:
  - Angular
  - Java EE
  - Spring
tags:
  - Angular
  - Java EE
  - Spring
---
**How to refresh an Angular 2 page using Java to avoid the 404 error**

You just built your first application using Angular 2 (or 4) and the tester comes to you saying that when he refreshes the page using the browser he get a 404 error. How it comes?

Angular doesn’t use anymore the # in the URL. When you refresh the page using the URL showed by Angular the web server doesn’t find any valid match.

**web.xml solution**

If you are developing your application with Java EE or Spring the solution is very simple and you don’t have to implement code in your frontend. You can simply add this code snippet in your _web.xml_

    <error-page>
        <error-code>404</error-code>
        <location>/</location>
    </error-page>
    

If the page is not found a simple redirection to the frontend page get the work done. The frontend will show the correct page.

**Using a controller**

A different approach uses a Spring `@controller` on the backend. In this example all the links (controllers) of the frontend are prefixed with ‘app’.

The controller receives the request and forward it to the frontend that open the correct page. This solution is implemented in the example: [angular.cafe](http://angular.cafe)

    package ch.javaee.demo.angular2.web;
    
    import org.springframework.stereotype.Controller;
    import org.springframework.web.bind.annotation.RequestMapping;
    
    @Controller
    public class RouterController {
    
        @RequestMapping({"/app-*"})
        public String app() {
            return "forward:/index.html";
        }
    }
    

**Using a Filter**

We only mention the possibility to solve the problem implementing a custom `javax.servlet.Filter`.