---
id: 794
title: New Angular 2 / Java Docker image
date: 2017-01-24T00:47:00+00:00
author: Marco Molteni
layout: post
main-class: 'angular'
guid: http://javaee.ch/?p=794
permalink: /2017/01/24/new-docker-image/
categories:
  - Angular
  - docker
  - example
  - Spring Boot
  - tutorial
---
<p dir="auto">
  Quick post to announce the new docker image of the angular / spring boot example.
</p>

<https://hub.docker.com/r/javaee/angular2-java-hello-world/>

Here the command to install locally: `docker pull javaee/angular2-java-hello-world`

The app is still very simple and it is visible here: <http://angular.cafe>

You can start the docker image with the following command:
  
`docker run --name angular -d -p 8080:8082 javaee/angular2-java-hello-world`

The app will be visible on <a href="http:// http://localhost:8080" target="_blank">http://localhost:8080</a>

I will add soon new features with the comments that explains how to implement them:
  
&#8211; Spring security
  
&#8211; Database
  
&#8211; REST unit tests
  
&#8211; Forms
  
&#8211; Complex pagination
  
&#8211; Machine learning

If you want to see something implemented you can write a comment.