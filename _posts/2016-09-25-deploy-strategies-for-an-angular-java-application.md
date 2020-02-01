---
id: 52
title: Deployment strategies for an Angular / Java application.
date: 2016-09-25T15:47:24+00:00
author: Marco Molteni
layout: post
guid: http://ngjava.com/?p=52
permalink: /2016/09/25/deploy-strategies-for-an-angular-java-application/
categories:
  - Intermediate
  - Uncategorized
---
<span style="font-size: small;">We want to show how to deploy an application that uses Java as back end and Angular as front end.<br /> </span><span style="font-size: small;">It’s a common problem to correctly integrate the 2 technologies because they have their own building system.<br /> </span><span style="font-size: small;">You have many options for the deployment.</span>

## Option 1. Java .war in 1 server

<span style="font-size: small;">We use the traditional approach and we deploy our application in a Java EE / Servlet server.</span>

<span style="font-size: small;">The final maven build should place correctly the web resources as showed in the image:</span>

[<img class="" style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="Angular directory structure" src="https://i1.wp.com/marco.dev/wp-content/uploads/2016/09/Angular-directory-structure_thumb.png?resize=204%2C470" alt="Angular directory structure" border="0" data-recalc-dims="1" />](https://i1.wp.com/marco.dev/wp-content/uploads/2016/09/Angular-directory-structure.png)

<span style="font-size: small;">In this case we called the package ROOT.war to deploy the package directly in the context root (e.g. </span>[<span style="font-size: small;">http://localhost:8080/</span>](http://localhost:8080/)<span style="font-size: small;">).</span>

## Option 2. Multiple servers

<span style="font-size: small;">It’s even possible to manage the front end and the back end separately.<br /> </span><span style="font-size: small;">For the development this is maybe the easiest solution (node.js for the web and tomcat for the back end services).<br /> </span><span style="font-size: small;">The 2 servers can be on the same machine.</span>

[<img class="" style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border: 0px;" title="angular_2_servers" src="https://i0.wp.com/marco.dev/wp-content/uploads/2016/09/angular_2_servers_thumb.png?resize=216%2C240" alt="angular_2_servers" border="0" data-recalc-dims="1" />](https://i0.wp.com/marco.dev/wp-content/uploads/2016/09/angular_2_servers.png)