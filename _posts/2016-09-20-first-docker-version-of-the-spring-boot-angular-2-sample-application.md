---
id: 749
title: 'First docker version of the Java Spring Boot and Angular 2 &#8216;Hello World&#8217; tutorial'
date: 2016-09-20T22:17:48+00:00
author: Marco Molteni
main-class: 'cloud'
layout: post
guid: http://ngjava.com/?p=38
permalink: /2016/09/20/first-docker-version-of-the-spring-boot-angular-2-sample-application/
categories:
  - Uncategorized
---
I updated the basic example that you can find here: <http://javaee.ch/2016/02/23/spring-boot-angularjs-2-typescript-hello-world-tutorial/>

Now it is compatible with docker. You can download the first version of the dockerfile here:

<https://github.com/marco76/SpringAngular2TypeScript/blob/master/Dockerfile>

It&#8217;s only a first test and a lot of work is still needed but you can build it:

<p class="p1">
  <span class="s1">docker build -t my-java-app .</span>
</p>

<p class="p1">
  The source code and the dependencies are downloaded. When the build end you can launch the application:
</p>

<p class="p1">
  <span class="s1">docker run &#8211;rm -it -p 8080:8080<span class="Apple-converted-space">  </span>my-java-app java -jar /usr/src/myapp/</span><span class="s2">angular2.jar </span>
</p>

<img class="alignnone wp-image-39 size-large" src="https://i1.wp.com/javaee.ch/wp-content/uploads/2016/09/davis_1.png?resize=945%2C221" alt="davis_1" data-recalc-dims="1" />

You can see the result on localhost:8080

<img class="alignnone size-medium wp-image-40" src="https://i1.wp.com/javaee.ch/wp-content/uploads/2016/09/davis_2.png?resize=300%2C141" alt="davis_2" data-recalc-dims="1" />

&nbsp;

Many people asked how to compile the project in eclipse &#8230; well, no need of eclipse or other IDE.
  
The procedure of creation of a unique war/jar is a bit complicated at the moment but only maven and npm are used. Typescript and npm compile the Angular files. Maven compile the java classes and build the jar/war including the backend (java) and frontend (javascript).

Personally I think that the frontend and backend should be developed in 2 separated projects.