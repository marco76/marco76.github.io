---
title: 'Spring Boot 2, Angular 5, Angular Material: Hello World'
description: 'Quickly start your new Spring Angular Application'
date: 2017-09-09T23:49:48+00:00
author: Marco Molteni
layout: post
main-class: 'spring'
color: '#7AAB13'
permalink: /spring-boot-angular/
categories:
  - Spring Boot
  - Angular
  - Java
  - Angular Material
  - REST
tags:
  - spring
  - angular

image: '/assets/img/'

introduction: 'Quickly start your new Spring Angular Application'
---

# Quick start your new Angular 5 (beta) / Spring Boot 2 (beta) project

This is a simple bootstrap for a web application based on Angular 5 (beta), Spring Boot 2 (beta), Angular Material.

The application gives you a structure to start a new project with a simple REST service called from the frontend.

Some features:
 - support for Angular CLI.
 - Swagger UI installed
 
 In the future I will simply fix the configuration and update the dependencies.
 No feature will be added.
 
 
 ## Build and run the project
 
You can find the project here:
  
https://github.com/marco76/spring-angular-material

to run it
 
 ```
 git clone https://github.com/marco76/spring-angular-material
 cd ./spring-angular-material mvn package
 java -jar ./target/ng-spring-full-app-0.0.1-SNAPSHOT.war
 
 You can access the application on http://localhost:8080
 ```
 <img src="{{site.baseurl}}/assets/img/uploads/2017/09/2017-09-09_23-26-04.png" alt=""/>]({{site.baseurl}}/assets/img/uploads/2017/09/2017-09-09_23-26-04.png)
    <img src="{{site.baseurl}}/assets/img/uploads/2017/09/2017-09-09_23-30-57.png" alt=""/>]({{site.baseurl}}/assets/img/uploads/2017/09/2017-09-09_23-30-57.png)

 ## Build and run in development
 
 During the development you have to start the frontend and the backend separately.
 
 The frontend runs on node.js, you can start with `ng serve`: http://localhost:4200
 The frontend runs on Tomcat embedded: http://localhost:8080